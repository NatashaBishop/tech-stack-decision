Conversation with google.com/ai
##  Technical Specification: Marketplace Communication Hub
Target Infrastructure: Standard cPanel Hosting (WSGI / Phusion Passenger)  
Database Engine: PostgreSQL (Indexed)  
Concurrency Target: 2,000 Active Users  
Core Architecture Principle: Passive, On-Demand Execution (Zero Persistent Connections)  
1. System Topology & Logic  

   [System/App Events] ───► (Auto-Generate DB Rows) ───┐
                                                       ▼
   [User Web Actions] ────► (Manual HTMX Request) ───► [ Django WSGI Server ] ──► [ PostgreSQL ]
                                                       │
                                                       └──► (SMTP Relay) ───────► [ User Email ]

To safely host 2,000 active users on cPanel, this architecture completely eliminates persistent server connections. Real-time streaming layers (WebSockets, ASGI, Server-Sent Events) and automated JavaScript background loops (setInterval) are forbidden.
The server operates on a strict request-and-release model. Connections open, execute database operations in milliseconds, and instantly terminate. When users are idle, server load drops to exactly 0%.
2. Technical Stack Boundaries
Approved Technologies

    Application Server: Django configured strictly in WSGI mode via Phusion Passenger (cPanel’s native "Setup Python App" tool).
    Database: PostgreSQL. All queries must hit indexed columns to maintain sub-5ms lookup speeds under load.
    Frontend Asynchrony: HTMX. A lightweight HTML library that executes AJAX requests directly through HTML attributes, completely removing the need for custom JavaScript files.
    Email Engine: Standard cPanel SMTP loop or external transactional relay (e.g., SendGrid/Mailgun) initialized asynchronously inside Django views.

Prohibited Technologies

    Avoid: Django Channels, Daphne, Hypercorn, or Uvicorn. These require continuous backend daemons that violate cPanel’s process-killing security mechanisms.
    Avoid: WebSockets or Server-Sent Events (SSE). Keeping thousands of browser sockets open concurrently will instantly trigger a 508 Resource Limit Exceeded crash on cPanel servers managed by CloudLinux.
    Avoid: Custom JavaScript background polling or tracking scripts.

3. Database Schema Blueprint
The PostgreSQL schema relies on indexed foreign keys and an explicitly typed message category system to separate human conversations from platform notices.

   ┌──────────────────┐          ┌──────────────────┐
   │   Conversation   │          │     Message      │
   ├──────────────────┤          ├──────────────────┤
   │ id (PK)          │◄─────────┤ conversation_id  │
   │ listing_id       │          │ sender_id (Null) │
   │ is_system_thread │          │ message_type     │
   │ created_at       │          │ text             │
   └──────────────────┘          │ is_read          │
                                 │ created_at       │
                                 └──────────────────┘

Entities & Relationships

    Conversation Table
        Tracks threads between buyers and sellers, or dedicated system channels.
        listing_id: Integer/Foreign Key linking the thread directly to a specific marketplace product listing.
        is_system_thread: Boolean flag. If True, the thread represents a direct, read-only communications line from the marketplace to the user.
    Message Table
        Stores individual items inside a thread.
        sender_id: Foreign Key linking to Django’s User model. Set to null=True, blank=True to allow the system application itself to act as the sender.
        message_type: CharField with strict choices: USER (Human message) or SYSTEM (Automated platform notice).
        is_read: Boolean flag used to drive all frontend interface badges. [1]

4. Module & Feature Specifications
Module A: The Marketplace Inbox (eBay Layout)

    Functional Description: A centralized dashboard listing all conversations involving the logged-in user, sorted chronologically by the latest activity.
    UI Rules & Variations:  
        User-to-User Threads: Displays the marketplace listing title, an image thumbnail of the product, the counter-party's username, and a truncated snippet of the text.  
        System-Automated Threads: Displays the platform's brand logo instead of a user avatar. The sender identity defaults to an official designation (e.g., "Marketplace System" or "Order Tracking"). The background container uses distinct branding colors to differentiate it from peer communication.
    Server Guardrails: Database pagination is strictly locked at 20 thread summaries per page. This prevents the server from scanning legacy historical rows when a user checks their inbox.  

Module B: The Global Navigation Bar Badge

    Functional Description: A persistent message icon or notification dot visible across every page of the web application. [1]
    Zero-Load Execution Rules: The notification state is evaluated only when a user naturally performs an action (loads a page, updates a search filter, or logs in). It never queries the backend dynamically while a page sits open.
    Backend Logic: Handled via a Django Context Processor executing a highly optimized PostgreSQL .exists() statement. It runs a single boolean check across the Message table where receiver == current_user and is_read == False.

Module C: The Chat Interface & Manual AJAX Refresh

    Functional Description: A split-screen window showing historical chronological messages inside a selected thread, featuring a bottom text input field.
    On-Entry Read Mechanics: Opening this view immediately updates all unread flags where the current user is the target recipient to is_read = True.
    The Manual AJAX Check: Users pull down new updates without a full page refresh using a manual button marked "Check for New Messages".
    HTMX Integration: Clicking this button uses hx-get to target an isolated view. Django looks for new rows created since the current page instance loaded, rendering only the raw HTML snippet of those specific new items. HTMX appends these to the bottom of the message container (hx-swap="beforeend").
    System Thread Adaptations: If the current thread has is_system_thread == True, the HTML text input box, file attachment tools, and "Send" button are hidden from the DOM. The interface transforms into a clean, read-only transaction ledger.

Module D: Out-of-App Email Notifications

    Functional Description: Automated notification emails dispatched to a user's regular inbox when they receive communications on the platform. [1]
    Differentiated Dispatch Rules:
        User-to-User Cool-Down: To prevent spamming the server's mail queue during an active chat session, an email alert is withheld if a user is online, or if an email was already sent for that thread within the last 10 minutes.
        Critical System Event Priority: Automated notices representing direct transactional shifts (e.g., "Payment Confirmed", "Offer Accepted", "Account Security Alert") bypass all cool-down thresholds. Django views invoke the mail server instantly upon database write to ensure immediate message delivery. [1]

5. System Execution Flows
Scenario 1: Buyer Sends a Message to a Seller

    Buyer enters text in the chat window and clicks "Send".
    An HTMX background POST request sends the payload to Django.
    Django writes a record to the Message table with message_type='USER'.
    The view returns a clean HTML fragment of the single new message. HTMX swaps it into the buyer's view instantly without refreshing the main navigation bar.
    The seller's global navigation badge remains unchanged until they naturally click to a new page or intentionally tap "Check for New Messages" inside that chat window.  

Scenario 2: System Generates an Order Update Notice  

1. A buyer clicks "Confirm Purchase" on a listing page.
2. The core checkout view updates the item state, then invokes a internal notification utility method.
3. This utility automatically writes a row to the Message table with sender_id=None and message_type='SYSTEM'.
4. The system concurrently calls Django's email layer, immediately pushing a transaction receipt email via the SMTP relay.
5. The next time the seller shifts pages or opens their mailbox, the system thread highlights itself with the official platform layout. [1]
