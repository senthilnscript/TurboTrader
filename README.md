# TurboTrader Gemini CLI Extension

TurboTrader connects Gemini CLI to the remote TurboTrader MCP server over Streamable HTTP. The server supports TTC and BOW trading brokers and routes requests using the broker and account authorized on the server.

## Installation

After this repository is pushed to GitHub, install the extension with:

```sh
gemini extensions install https://github.com/senthilnscript/TurboTrader.git
```

From the extension directory, validate the local package with Gemini CLI 0.68.0:

```sh
gemini extensions validate .
```

## Authentication

The extension contains no API keys, OAuth client secrets, broker credentials, or access tokens. Gemini CLI uses remote MCP OAuth discovery and a public dynamically registered OAuth client; authentication and tokens are handled by Gemini CLI and the TurboTrader server.

TurboTrader requires a connected TTC or BOW broker session and user authorization for the requested scopes. Its Gemini account-linking flow uses a short-lived code generated from TurboTrader's **Connect AI** flow; enter that code in the TurboTrader consent page when prompted.

**Known compatibility issue:** Gemini CLI 0.68.0 uses a local loopback OAuth callback (`http://localhost:<port>/oauth/callback`). The current TTC.MCP production dynamic-client registration accepts Gemini Spark's Google-hosted callback URLs instead. As a result, the extension's manifest and transport are compatible, but OAuth registration/authentication is expected to fail until the server adds the Gemini CLI callback flow. The MCP server was not changed for this package.

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

## Verify the installation

```sh
gemini extensions list
gemini mcp list
```

Gemini CLI 0.68.0 also supports `/mcp auth turbotrader` in an interactive session to start remote OAuth. The current server callback mismatch described above must be resolved before this authentication step can complete.
