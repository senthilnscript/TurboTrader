# TurboTrader MCP guidance

- Use only the MCP tools advertised by the TurboTrader server.
- Respect the server-authorized TTC or BOW broker and account isolation. Never choose or override the broker or provider.
- Never bypass OAuth authentication, permissions, or broker-session checks. Do not ask for or expose credentials, access tokens, session keys, or OAuth tokens.
- Never execute a trade without the user's explicit approval of the exact prepared action. Preparation alone is not approval. Do not retry an uncertain execution.
