---
name: google-ads-queries
description: Use when the user asks anything about their Google Ads data (spend, campaigns, search terms, keywords, conversions, wasted budget, performance over time) and the ads-mcp tools are available. Picks the right ads-mcp tool and writes GAQL that works the first time.
version: 1.0.0
---

# Google Ads questions with ads-mcp

Every ads-mcp call that reads the account uses one call of the user's monthly plan (the free plan has 15). `list_accounts`, `plan_status` and `why_not_upgrade` are free. So the goal is the answer in as few calls as possible, and no failed calls.

## Pick the tool before writing GAQL

| The user wants | Tool | Calls |
|---|---|---|
| Where money is wasted, what to cut, why costs are high | `find_wasted_spend` | 1 |
| What people typed before clicking | `search_terms` | 1 |
| How each campaign did | `campaign_performance` | 1 |
| A negative keywords file | `negative_keywords_file` (Unlimited) | 1 |
| Did a change on a given day help | `did_it_work` (Unlimited) | 1 |
| A report for a client | `client_report` or `client_dashboard` (Unlimited) | 1 |
| Daily metrics to keep | `export_history` (Unlimited) | 1 |
| Anything else | `run_gaql_query` | 1 per query |

Use `run_gaql_query` only when no tool above covers the question. Plan the query, then send it once.

## GAQL that works the first time

- Shape: `SELECT field, field FROM resource WHERE ... ORDER BY ... LIMIT n`. There is no `SELECT *`: name every field.
- Only `SELECT` runs. The server is read-only.
- Money fields end in `_micros`: divide by 1,000,000 to get the account currency. `metrics.cost_micros` 12500000 is 12.50.
- Dates: `WHERE segments.date DURING LAST_30_DAYS`, or `WHERE segments.date BETWEEN '2026-09-01' AND '2026-09-30'`. Add `segments.date` to the SELECT only if you want one row per day.
- Text and enum values go in single quotes: `campaign.status = 'ENABLED'`. To skip deleted items: `campaign.status != 'REMOVED'`.
- The tool returns the first 500 rows. Always add `ORDER BY metrics.cost_micros DESC` (or another order) and a `LIMIT`, so the 500 are the ones that matter.
- Pick the resource that owns the question: `campaign`, `ad_group`, `keyword_view` (keywords with metrics), `search_term_view` (search terms), `ad_group_ad` (ads), `campaign_budget`, `conversion_action`, `customer`. Fields from a resource that cannot be joined with the one in FROM make the query fail.
- Client accounts under a manager account need `login_customer_id`; get it from `list_accounts`.

## When a query fails

Read the error: Google names the field or clause that is wrong. Fix that one thing and retry once. If it fails again, tell the user what you tried instead of guessing more field names: each retry is another monthly call. An `UNRECOGNIZED_FIELD` error usually means the field was renamed or removed in the current API version; the field reference is at developers.google.com/google-ads/api/fields.

## Plan limits

If a tool returns a plan limit message, show it to the user as it is, with its link, and stop calling ads-mcp tools. Retrying returns the same message.
