# TurboTrader Gemini CLI and Antigravity Extension

TurboTrader connects Gemini CLI and Google Antigravity to the remote TurboTrader MCP server over Streamable HTTP. The server supports TTC and BOW trading brokers and routes requests using the broker and account authorized on the server.

## Gemini CLI installation

After this repository is pushed to GitHub, install the extension with:

```sh
gemini extensions install https://github.com/senthilnscript/TurboTrader.git
```

To validate the local package with Gemini CLI 0.68.0:

```sh
gemini extensions validate .
```

## Google Antigravity installation

After this repository is available locally or pushed to GitHub, install the plugin from its repository directory with the Antigravity CLI:

```sh
agy plugin validate .
agy plugin install .
agy plugin list
```

The plugin bundles its MCP server configuration in `mcp_config.json`. For the Antigravity App, use **Customizations > Installed** to inspect the installed plugin and authenticate its MCP server.

## Authentication

The extension contains no API keys, OAuth client secrets, broker credentials, or access tokens. Both clients must use the server's OAuth flow; authentication, permissions, and account isolation remain server controlled. TurboTrader requires a connected TTC or BOW broker session and user authorization for the requested scopes. Its Gemini account-linking flow uses a short-lived code generated from TurboTrader's **Connect AI** flow; enter that code in the TurboTrader consent page when prompted.

**Known OAuth compatibility concern:** The current TTC.MCP production OAuth registration validates client redirect URIs against its configured client policies. Its dynamic registration currently permits Gemini Spark's Google-hosted callback set, Claude callbacks, and ChatGPT callbacks. Antigravity's documented OAuth callback is `https://antigravity.google/oauth-callback`, which is not among the current accepted callback policies. Therefore, Antigravity can load the remote MCP endpoint, but OAuth sign-in is expected to fail until the server explicitly supports Antigravity's CIMD/DCR client and callback. The server was not changed for this package. Do not add a static secret or token to work around this limitation.

## Available MCP functionality

The server advertises tools for:

- Public supported-broker discovery.
- Authenticated session status, account profile, funds, margin usage, holdings, positions, order books, and trade books.
- Symbol search, quotes, and order-margin checks.
- Preparing orders, modifications, cancellations, position square-off, and product conversion; executing supported prepared actions after explicit user approval.
- Checking a prepared action's approval status.

Availability depends on broker/provider capability and OAuth scopes. The server controls broker selection; the model cannot switch providers by changing tool arguments.

## Security

- Keep OAuth, broker, and session credentials out of this repository, prompts, and logs.
- Do not bypass server-side OAuth, scope checks, or session validation.
- Preparation does not submit an order. Require the user's explicit approval of the exact prepared action before execution, and do not retry an uncertain execution.
- The server enforces TTC/BOW and account isolation. Use only the tools the server advertises.

## Verify installation

Gemini CLI:

```sh
gemini extensions list
gemini mcp list
```

Gemini CLI 0.68.0 also supports `/mcp auth turbotrader` in an interactive session to start remote OAuth. The current server callback mismatch described above must be resolved before Gemini CLI's authentication step can complete.

Antigravity:

```sh
agy plugin list
```

In the Antigravity App, check **Customizations > Installed > MCP Servers** for server status and the **Authenticate** action. OAuth compatibility still depends on the server accepting Antigravity's callback.
