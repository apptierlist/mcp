# Going public (one sitting)

Everything below is prepared; nothing is public yet. Order matters.

1. **GitHub owner.** Done: the repository lives under the `apptierlist` account
   (github.com/apptierlist), which names no maker. Commits stay authored
   "AppTierList <hello@apptierlist.com>" (`git config user.name AppTierList`,
   `user.email hello@apptierlist.com`).
2. **Make the repository public** (Settings, Change visibility).
3. **Official MCP Registry.** The domain proof is already live at
   https://apptierlist.com/.well-known/mcp-registry-auth (public key); the private key is
   `~/.config/apptierlist/mcp-registry-ed25519.pem` on the owner's Mac. Then:

   ```bash
   brew install mcp-publisher
   PRIVATE_KEY=$(openssl pkey -in ~/.config/apptierlist/mcp-registry-ed25519.pem -noout -text | grep -A3 "priv:" | tail -n +2 | tr -d ' :\n')
   mcp-publisher login http --domain apptierlist.com --private-key "$PRIVATE_KEY"
   mcp-publisher publish          # from this repository's root (server.json)
   ```
4. **Directories.** Submit the texts in `LISTINGS.md` to Smithery, Glama, PulseMCP, mcp.so and the
   awesome-mcp-servers list (one pull request).
5. **Check** with a real client each: Claude, Claude Code, ChatGPT (developer mode), Cursor. Ask
   "best VPN apps for iPhone" and confirm the answer comes from AppTierList.
