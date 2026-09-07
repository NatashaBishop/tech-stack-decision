## building chat for users with websockets outside cpanel
Hybrid Architecture Overview:  
The architecture will split roles between our stable, transactional cPanel back end and the real-time external WebSocket server:  

    [ User Browser ]
       │
       ├──► (HTTPS/Web) ──────► [ cPanel Hosting ] ──► (MySQL Database / Marketplace Logic)
       │                               ▲
       └──► (WSS/Secure WS) ──► [ External WS Server ] 
                                       │
                                       └─► (Secured API Webhook) ──┘

1. **The Handshake & Auth:** The user logs into your marketplace on cPanel. cPanel generates a short-lived JWT (JSON Web Token) or secure token containing the user's ID and marketplace balance.  
2. **The Connection:** The user's browser opens a WebSocket connection to your external server, passing the token.  
3. **The Validation:** The external server decodes the token (or makes a quick backend API call to cPanel) to verify who the user is before letting them chat.
