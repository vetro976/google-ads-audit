# Google Ads Audit (Ads MCP)

The `ads-mcp` tools read Google Ads accounts. They can never change them: no bids, no budgets, no edits.

- Start with `list_accounts` (it does not use a monthly call). For a client account under a manager account, pass the `login_customer_id` it shows.
- For "where is my money going" questions, call `find_wasted_spend` once. It already ranks the search terms, keywords and campaigns that spent without converting, and checks conversion tracking and disapproved ads. Do not rebuild it with many `run_gaql_query` calls.
- You do not know what the business sells. Before calling a search term waste, ask.
- Never state a number that did not come from a tool result.
- If a tool returns a plan limit message, show it as it is and stop calling ads-mcp tools.

The first call opens a Google sign-in for Ads MCP. Free plan: 15 tool calls a month. Details at https://adsmcp.dev
