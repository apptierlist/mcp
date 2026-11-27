# Client install tests

Each line is one real run: a client given only this repository's README, asked to set up the
server and answer a question with it.

| Date | Client | Model | Result |
|---|---|---|---|
| 2026-11-27 | Cline CLI 3.0.76 | Claude Haiku 5.5 | Pass. From the README alone it wrote `{"mcpServers":{"apptierlist":{"url":"https://apptierlist.com/mcp"}}}` to its MCP settings, called `search_apps` ("vpn") and `get_category` ("vpn"), and returned the top three VPN apps. |
