I am making a decision on tech stack and tools for my marketplace. Making architectural decision to avoid being stack at later stage.  

            I am looking for an AI model to code my cashless bartering marketplace.  
            Some features are already done by ex-CTO (with no AI help) in Django with MySQL as a database.  
            I am looking into continuing along (without CTO) with my project, with a help of AI.  
            I am looking into AI agents to choose one to help me with my project. I need to make few decisions:  
               1. To use existing pre-AI code base (Django + MySQL) or  
               2. Start from scratch with Django + PostgreSQL or  
               3. Start from scratch with new stack which is more suitable for a chosen AI model.  
            I also need to make a decision on an AI model to use to build my my cashless bartering marketplace.  
            It will have an internal tocken, but only on a database level, not a cripto.  
### To build a cashless bartering marketplace I am choosing Option 2 (Starting from scratch with Django + PostgreSQL).  
AI models are exceptionally well-trained in Django because it is an established, opinionated framework. Swapping MySQL for PostgreSQL is a critical upgrade for a bartering marketplace. PostgreSQL offers native ACID-compliant transactions and row-level locking. This prevents "race conditions"—such as a user accidentally double-spending their internal ledger token if they click a "buy" button twice simultaneously.
### Some questions arised: 
1. How complex is my frontend design (e.g., do I want a simple web layout or a flashy mobile-first app)?
   
            I am after the most simple, user friendly interface.
            Not to impress, but let the user understand the functionality straight away.
            Simple, informative, intuitive.
   
3. What is my comfort level with running terminal commands locally?
   
            I am a confident user of VScode and terminal. Not a complete beginner, but not an engineer.
   
4. What do I need to understand next?
   
            I would like to understand what tools to use and how to do an initial setup. I like VScode.
            I have a hosting with cPanel where Django is already setup and running the old code (legacy Django + mySql). 
            I see a facility on my cPanel to create postgreSQL. I will do that. 
            I also would like to use a git for backing up the project. 
            If it is possible to make my Github account work synchronously with my cPanel hosting. 
            It would be great to come up with a stack of tools that let me code locally on my machine 
            and run preview locally before I push to github and hosting.

### Concidered Tool Stack
- AI Assistant: https://blitzy.com/#pricing Reverse Engineer existing code base
- AI Assistants: VSCode + Install the Continue extension inside VSCode (see file continue-extension-in-VSCode.md), and connect it to Anthropic's Claude. It allows the AI to read my files, run terminal commands and write code directly in my local project workspace with your permission.
- Frontend Design: Tailwind CSS compiled directly into a static, minified file, paired with HTMX for dynamic interactivity.  
  Why: The most efficient, high-performance architecture for a high-load Django + Postgres project hosted on cPanel.  
  If project efficiency and performance under high load are top priorities, we must absolutely avoid using browser-based CDNs (like the tailwindcss/browser script  
- Local Database: Docker Desktop (Running PostgreSQL)  
  Why: Since cPanel uses Linux-based PostgreSQL, installing PostgreSQL directly on a Windows/Mac machine can lead to version mismatches. Installing Docker Desktop allows me to spin up an identical local PostgreSQL database with a single terminal command.
- Deployment Sync: GitHub Actions  
  Why: Do not upload files manually via cPanel file manager. I will set up a automated pipeline: when I push code to GitHub, GitHub will automatically securely deploy the updates to my cPanel hosting.
### Choosing AI agents for my project
Because I do not have a CTO, I need an autonomous AI agent that can look at the entire project directory, create database migrations and run tests (not just a basic chat window)  

1. Claude Code / Cursor (Powered by Claude 4.7 Sonnet) — The Best Choice is Claude Code at the moment (or the Cursor IDE editor running Claude) is the absolute industry gold standard for backend heavy frameworks like Django.  
2. Why Claude Code fits: It has a massive 1-million-token context window. This means it can read the entire Django repository (all models, views, and configurations) at the same time.
3. How it handles the token: Claude excels at deep, linear architectural reasoning. It is less likely to introduce bugs into the internal token ledger mechanics.  

            ┌────────────────────────────────────────────────────────┐  
            │                      THE FINAL STACK                   │  
            ├─────────────┬──────────────────────────────────────────┤  
            │ Backend     │ Django (Python)                          │  
            ├─────────────┼──────────────────────────────────────────┤  
            │ Database    │ PostgreSQL (Managed via cPanel)          │  
            ├─────────────┼──────────────────────────────────────────┤  
            │Interactivity│ HTMX (Zero-build JS via CDN)*            │  
            ├─────────────┼──────────────────────────────────────────┤  
            │ Styling     │ Tailwind CSS (Compiled Locally via CLI)  │  
            └─────────────┴──────────────────────────────────────────┘  
            * see HTMX.md file
### Why this stack sutable 4 cPanel:  
- No Node.js on the Server: cPanel handles Python (via WSGI) and PostgreSQL brilliantly. It struggles heavily with Node.js processes (like running a React server or an active Tailwind compiler). This stack keeps Node.js 100% on your local computer.
- No API Overhead: You do not need to build, secure, or maintain a Django REST Framework API because HTMX talks directly to standard Django views.
- Native Security: Django’s built-in CSRF tokens and user session cookies protect your digital wallet ledger out of the box.

