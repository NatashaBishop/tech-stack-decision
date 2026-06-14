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
   
4. What do I need to understand next?
   
         I would like to understand what tools to use and how to do an initial setup. I like VScode. I have a hosting with cPanel where Django is already setup and running the old code (legacy Django + mySql). I see a facility on my cPanel to create postgreSQL. I will do that. I also would like to use a git for backing up the project. If it is possible to make my Github account work synchronously with my cPanel hosting. It would be great to come up with a stack of tools that let me code locally on my machine and run preview locally before I push to github and hosting.

### Concidered Tool Stack
- AI Assistant: VSCode + Roo Code extension (using Claude 3.7 Sonnet / 3.5 Sonnet)  
   Why: Since you already use VSCode, do not switch to a whole new editor. Install the free Roo Code (formerly Roo Cline) or Continue extension inside VSCode, and connect it to Anthropic's Claude. It allows the AI to read project files, run terminal commands, and write code directly in my local project workspace with my permission.
- Frontend Design: Tailwind CSS via CDN  
  Why: To keep the UI simple, intuitive, and informative, use Tailwind CSS. It allows the AI to style elements inline using utility classes (e.g., <div class="bg-white p-6 rounded-lg shadow-md">). It requires zero complex build tools; you just drop a single script link into my Django HTML base template.
- Local Database: Docker Desktop (Running PostgreSQL)  
  Why: Since cPanel uses Linux-based PostgreSQL, installing PostgreSQL directly on a Windows/Mac machine can lead to version mismatches. Installing Docker Desktop allows you to spin up an identical local PostgreSQL database with a single terminal command.
- Deployment Sync: GitHub Actions  
  Why: Do not upload files manually via cPanel file manager. You will set up a automated pipeline: when you push code to GitHub, GitHub will automatically securely deploy the updates to my cPanel hosting.
### Choosing AI agents for my project
Because I do not have a CTO, I need an autonomous AI agent that can look at the entire project directory, create database migrations and run tests (not just a basic chat window)  

1. Claude Code / Cursor (Powered by Claude 4.7 Sonnet) — The Best Choice is Claude Code at the moment (or the Cursor IDE editor running Claude) is the absolute industry gold standard for backend heavy frameworks like Django.  
2. Why Claude Code fits: It has a massive 1-million-token context window. This means it can read the entire Django repository (all models, views, and configurations) at the same time.
3. How it handles the token: Claude excels at deep, linear architectural reasoning. It is less likely to introduce bugs into the internal token ledger mechanics.
