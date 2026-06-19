### Notes on optimasing AI token usage  

For AI model: 
- To control expenses, implement prompt caching (available on Claude models) to lower input costs by up to \(90\%\) for repetitive tasks
For Django:
- a Django middleware snippet that hooks directly into the views to log actual token usage metrics straight to a PostgreSQL database table (AI tokens used by user)
