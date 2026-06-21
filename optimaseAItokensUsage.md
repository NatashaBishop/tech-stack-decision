### Notes on optimasing AI token usage  

For AI model: 
- To control expenses, implement prompt caching (available on Claude models) to lower input costs by up to \(90\%\) for repetitive tasks
For Django:
- a Django middleware snippet that hooks directly into the views to log actual token usage metrics straight to a PostgreSQL database table (AI tokens used by user)
- Isolate Context: When moving to Phase 4 (APIs), do not feed Fable 5 the entire testing suite. Only give it the models and services it needs to see.
- Set Output Guardrails for AI agent: Explicitly tell the model: "Skip long introductory explanations. Write the code immediately and only add comments for highly complex logic."Use Context Caching: keep the base project description and core models.py cached so you do not pay full price to resend them in every prompt.
