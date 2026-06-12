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
   I am after the most simple, user friendly interface. Not to impress, but let the user understand the functionality straight away. Simple, informative, intuitive.  
2. What is my comfort level with running terminal commands locally?
   I am a confident user of VScode and terminal. Not a complete beginner, but not an engineer.
3. What do I need to understand next?
   I would like to understand what tools to use and how to do an initial setup. I like VScode. I have a hosting with cPanel where Django is already setup and running the old code (legacy Django + mySql). I see a facility on my cPanel to create postgreSQL. I will do that. I also would like to use a git for backing up the project. If it is possible to make my Github account work synchronously with my cPanel hosting. It would be great to come up with a stack of tools that let me code locally on my machine and run preview locally before I push to github and hosting.
