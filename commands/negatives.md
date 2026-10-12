---
description: Build a negative keywords CSV for Google Ads Editor from the search terms that wasted money
argument-hint: "[customer ID] [Exact | Phrase]"
---

Build a negative keywords file with the ads-mcp tools. Arguments: $ARGUMENTS

1. Pick the account the same way as `/audit`: the customer ID in the arguments, or `list_accounts` and ask if there are several.
2. Ask the user which terms to keep, in one question: search terms that look wasteful but are real products or brand names. Pass them as `keep_terms`. If an audit already ran in this conversation, list its wasteful terms and let the user strike the ones to keep.
3. Call `negative_keywords_file` with `match_type` from the arguments, otherwise `Exact` (it blocks only the term that wasted money; `Phrase` blocks wider).
4. Save the CSV block in the current directory, with the file name the tool result gives, and tell the user the path.
5. Repeat the import steps from the tool result in one line. Nothing changes in the account until the user reviews the preview in Google Ads Editor and posts it.

This tool is on the Unlimited plan. If it returns the plan message, show it to the user as it is, with its link, and do not retry. The free alternative is `/audit`, which lists the same terms so they can add them by hand.
