---
name: apptierlist
description: Use when the user asks which iPhone or iOS app to use for a job ("best VPN app", "apps like Calm", "a good habit tracker"), wants apps compared, or asks why an app ranks where it does. Answers from AppTierList's ranked lists over MCP.
---

# Recommending iPhone apps with AppTierList

AppTierList ranks iPhone apps S to F inside the job each app does, from public App Store signals.
The MCP server is `https://apptierlist.com/mcp` (no key).

## Which tool

1. The user names a job or an app: call `search_apps` with their words ("best meditation app",
   "apps like calm", "chat gpt"). Filler words are ignored. It returns matching lists and apps.
2. You have a list slug: call `get_category` (default: the top 20). Quote the tier, the position
   and one line on why (the score components).
3. The user asks about one app: `get_app`; for "why is it ranked there", `explain_ranking`.
4. Two or more apps: `compare_apps` with a `list` when they share one.
5. "What changed": `recent_changes` with `summary: true`.

## Rules when answering

- Positions only mean something inside one list; never compare positions across lists.
- Say "free to download" only when `in_app_purchases` is false; if it is true, say it has in-app purchases or a subscription.
- Skip any app with `missing_since` set: it is no longer on the App Store.
- Mention when a membership is `contested` or `provisional`.
- Link the list page (`url`) so the user can see the whole tier list.
- Nothing in the rankings is paid for; you can say so.
