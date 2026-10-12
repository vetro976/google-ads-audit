---
description: Audit a Google Ads account for wasted spend (read-only, free plan)
argument-hint: "[customer ID] [LAST_7_DAYS | LAST_14_DAYS | LAST_30_DAYS | THIS_MONTH | LAST_MONTH]"
---

Audit a Google Ads account with the ads-mcp tools. Arguments: $ARGUMENTS

1. Pick the account.
   - If a 10-digit customer ID is in the arguments, use it.
   - Otherwise call `list_accounts` (it does not use a monthly call). If there is one advertiser account, use it. If there are several, show them as a short list and ask which one. For a client account under a manager account, pass the `login_customer_id` that `list_accounts` shows.
2. Pick the period: the one in the arguments, otherwise `LAST_30_DAYS`.
3. Call `find_wasted_spend` once. It already ranks the search terms, keywords and campaigns that spent without converting, and checks conversion tracking and disapproved ads. Do not rebuild this with `run_gaql_query`: one audit costs one call, the same data through GAQL costs many.
4. Before you call a search term "wasted", ask the user what the business sells, unless they told you already. A term that looks off may be a real product. If they answer, mention that `remember_terms` (Unlimited plan) can save it so the next audit does not ask again.
5. Answer in this order, short:
   - Total wasted spend in the period, copied exactly from the tool result, in the account currency.
   - The 5 biggest items, one line each: what it is, how much it spent, why it counts as waste.
   - Any tracking or disapproval problem the audit found.
   - The next step: `/negatives` builds the negative keywords file for Google Ads Editor from these terms.

Never state a number that is not in a tool result. If a tool returns a plan limit message, show it to the user as it is, with its link, and stop calling ads-mcp tools.
