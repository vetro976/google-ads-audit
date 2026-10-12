---
description: Write a plain-words Google Ads report for a client, as text or a one-page HTML dashboard
argument-hint: "[customer ID] [text | dashboard]"
---

Write a client report with the ads-mcp tools. Arguments: $ARGUMENTS

1. Pick the account the same way as `/audit`.
2. Pick the format: `dashboard` in the arguments means `client_dashboard`, anything else means `client_report`. Period: `LAST_30_DAYS` unless the user says otherwise.
3. Call the tool once.
   - `client_report`: save the text as `report-<customer ID>-<YYYY-MM-DD>.md` in the current directory.
   - `client_dashboard`: save the HTML as `report-<customer ID>-<YYYY-MM-DD>.html`, and give the user the temporary link (it expires in 24 hours).
4. Tell the user the file path in one line, and the one number the client will care about most (wasted spend, copied exactly from the result).

The report is written for someone who does not do PPC: do not add jargon to it. This tool is on the Unlimited plan. If it returns the plan message, show it to the user as it is, with its link, and do not retry.
