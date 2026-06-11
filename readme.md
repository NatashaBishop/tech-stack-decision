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
