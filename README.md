# Google Ads Audit for Claude Code

Read-only Google Ads audits from your terminal. It connects Claude Code to [Ads MCP](https://adsmcp.dev), which can read your Google Ads accounts but can never change them: no bids, no budgets, no edits. When it finds a problem, it gives you a file you review and import yourself.

## Commands

| Command | What it does | Plan |
|---|---|---|
| `/google-ads-audit:audit [customer ID] [period]` | Ranks the search terms, keywords and campaigns that spent without converting, and checks tracking and disapproved ads | Free |
| `/google-ads-audit:negatives [customer ID] [Exact\|Phrase]` | Saves a negative keywords CSV ready for Google Ads Editor | Unlimited |
| `/google-ads-audit:report [customer ID] [text\|dashboard]` | Writes a plain-words report for a client, as Markdown or a one-page HTML dashboard | Unlimited |

The `google-ads-queries` skill also kicks in on any Google Ads question: it picks the right tool and writes GAQL that runs the first time, so you do not burn calls on failed queries.

## Setup

1. Install the plugin in Claude Code:
   ```
   /plugin marketplace add vetro976/google-ads-audit
   /plugin install google-ads-audit@adsmcp
   ```
2. The first time a command runs, Claude Code opens a Google sign-in for Ads MCP. Sign in with the Google account that can open your Google Ads accounts.

Free plan: 15 tool calls per month. Unlimited: $55.30/month for the first 10 subscribers, then $79/month. Details at [adsmcp.dev](https://adsmcp.dev).

If you already added the Ads MCP connector on claude.ai, its tools may show up twice in Claude Code. Both work the same.
