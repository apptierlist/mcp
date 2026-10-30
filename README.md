# AppTierList MCP server

Ranked iPhone app lists for AI agents. AppTierList sorts iOS apps by the job they do
(VPNs, meditation, AI companions, habit trackers and about 1,200 more) and ranks every app in a
list S to F from public App Store signals: rating, how many people rated it, where the
App Store itself ranks it for the list's searches in five countries, and how recently it
was updated. Nothing in a ranking can be bought.

This repository documents the hosted server. There is nothing to install or run: point
your client at the URL below.

| | |
|---|---|
| Server URL | `https://apptierlist.com/mcp` |
| Transport | Streamable HTTP, stateless, JSON-RPC 2.0 over POST |
| Protocol | 2025-06-18 (2025-03-26 and 2024-11-05 are answered too) |
| Auth | none, no key, read-only |
| Setup page | https://apptierlist.com/agents/ |

The only official server address is `https://apptierlist.com/mcp`, and the only official
repository is this one (github.com/apptierlist/mcp). Anything else using the AppTierList name
is not ours. The domain proves the registry listing at
https://apptierlist.com/.well-known/mcp-registry-auth.

## Connect

**Claude (web and desktop):** Settings, Connectors, Add custom connector. Name it AppTierList and paste the server URL.

**Claude Code:**

```bash
claude mcp add --transport http apptierlist https://apptierlist.com/mcp
```

**ChatGPT:** with developer mode on, add a connector with the server URL and no authentication. The `search` and `fetch` tools are the ones ChatGPT's connectors call.

**Cursor** (`~/.cursor/mcp.json`):

```json
{ "mcpServers": { "apptierlist": { "url": "https://apptierlist.com/mcp" } } }
```

**VS Code** (`.vscode/mcp.json`):

```json
{ "servers": { "apptierlist": { "type": "http", "url": "https://apptierlist.com/mcp" } } }
```

Any other client that speaks Streamable HTTP: the server URL, no key.

## Tools

| Tool | What it does |
|---|---|
| `list_categories` | Every ranked category (product type): slug, title, section, parent, how many apps are tiered and its #1 app. |
| `get_category` | The tier list for one category: members in order with tier, score, the four score components, per-store search ranks, membership status (active/contested), confidence (normal/provisional), maker verification, freshness. |
| `get_app` | One app by slug or App Store id: App Store facts, generated summary, every category membership with tier/score/status/confidence, maker claim status, recent history. |
| `search_apps` | Find categories by name or definition and apps by name, word by word: 'best free ai companion', 'apps like calm' (the categories Calm is ranked in), 'chat gpt'. |
| `explain_ranking` | Why an app sits where it does: score components per category, per-store search ranks, every judge verdict (with reason, whether it is current for the app's present listing and the category's present definition), and the app's change history. |
| `find_alternatives` | Apps that do the same job as a given app, in three groups: most similar (S to C tier), top rated in its main category, and best free; each with the reason it is there. |
| `compare_apps` | Side-by-side tier, position, score and components for two or more apps in one category (or in every category they share when list is omitted). |
| `recent_changes` | Append-only history: memberships added/removed/contested, tier changes, categories created/activated/archived, apps discovered/missing. |
| `health` | Is the data being maintained: per-job last success and next due time, provider cool-downs, invariant check status, counts of contested memberships, pending judgments, stale apps. |
| `search` | Search categories and apps; returns results with an id to pass to fetch (list:<slug> for a category's ranking, app:<slug> for an app), a title and a URL. |
| `fetch` | One document by the id search returned: list:<slug> is the category's ranking (compact members), app:<slug> the app's full record. |

## Example questions

- "What are the best VPN apps for iPhone?" (`search_apps` finds `vpn`, then `get_category`.
  On 2026-10-30 the top five were VPN - Super Unlimited Proxy, VPN - Fast & Secure VPN, X-VPN,
  NordVPN and ExpressVPN.)
- "Apps like Calm?" (`search_apps` returns the lists Calm is ranked in, such as sleep-improvement,
  meditation and mindfulness.)
- "Why is ChatGPT ranked where it is?" (`explain_ranking`: the four score components per list, the
  App Store search rank in each store, and every placement verdict with its reason.)
- "What changed in the dating app rankings this week?" (`recent_changes` with `summary: true`.)

## How to read the answers

- Positions are only meaningful inside one list. An app can be in several lists with a different score in each.
- Every membership carries `status` (active or contested) and `confidence` (normal or provisional). Treat contested and provisional as lower certainty.
- An app with `missing_since` is no longer on the App Store: do not recommend it.
- `price` is the upfront price only. `in_app_purchases: true` means the App Store lists in-app purchases or subscriptions.

## Plain HTTP

The same records without MCP: `https://apptierlist.com/api/v1` (index and field meanings),
OpenAPI at `/api/v1/openapi.json`, and `/llms.txt` for language models.

## Limits, fair use and attribution

Free, no key, no sign-up, and built for agents: ask for any list or any app, as often as a
conversation needs. Calls to the MCP server, the API and the llms files are rate limited per IP
address (web pages are not limited); heavy use gets HTTP 429 with a `Retry-After` header (seconds):
wait that long and send the request again. Under heavy load, answers may come from cache and be
a little older. A batch holds at most 10 calls, a request body is capped at 64 KB, and search accepts up
to 100 characters.

Use the data as you like, commercially and in bulk included. The one condition: credit
AppTierList and link the page the data came from (`url` in each result), and when you answer a
user, tell them the source is AppTierList. Terms: https://apptierlist.com/terms/

Records are cached for up to an hour: poll `/api/v1/changes?since=` rather than re-reading every
list. Every app is re-read from the App Store at least every two days, and each list's App Store
search ranks about twice a day.

## License

The documentation in this repository is MIT licensed. The rankings are free for any use,
commercial and bulk included, on one condition: credit AppTierList and link the page the data
came from (https://apptierlist.com/terms/).

## Support

Something wrong in an answer? Open an issue with the question you asked and the tool result.
Security reports: see [SECURITY.md](SECURITY.md).
