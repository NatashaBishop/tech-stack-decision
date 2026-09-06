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
