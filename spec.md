### Describe market place features here.  
ask AI model for help with this file - good idea.  

Step 1: Brainstorm the FeaturesFirst, tell Claude your raw ideas in plain, messy English. Let it do the heavy lifting of organizing them. Run this prompt: 

    "Claude, I want to build a white-label, multi-niche cashless barter marketplace engine using Django and PostgreSQL. My first niche is a general barter system, but later I want to support niche apps like gardening, books, and localized peer-to-peer parking exchanges. Based on this, please write a comprehensive, markdown-formatted spec.md file listing all the core features a functional MVP needs (such as token wallets, escrow, location tracking, and dynamic item listings)."  
Step 2: Establish the Code RulesOnce you are happy with the feature list, ask Claude to write its own engineering guardrails based on that exact spec. Run this prompt:  

    "Now that we have our spec.md, please generate a strict .claudeco-rules file optimized for a Django + PostgreSQL architecture. Make sure it explicitly forces developers to use 'Fat Models, Skinny Views', PostgreSQL transaction.atomic() for financial ledger data, and JSONB fields for infinite niche flexibility so the codebase never has to be rewritten."Step 3: Tell Claude to Save the FilesAfter Claude outputs the text for both files into your terminal, you can simply ask it to write them directly to your computer's hard drive:"Claude, please create and save these files in my project root directory as .claudeco-rules and spec.md."  
###  What to Look Out For:  
  When Claude generates these files, read through them quickly to make sure it didn't add unnecessary features you don't want yet (like complex email verification systems or paid subscription tiers). Keep the V1 features focused strictly on user accounts, listing assets, trading, and the token ledger
