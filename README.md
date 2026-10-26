<p align="center"><img src="logo.png" width="96" height="96" alt="readygpu logo"></p>

# readygpu MCP server

The remote [MCP](https://modelcontextprotocol.io/) server for [readygpu](https://readygpu.com): hardware for local AI.

Live prices in local currency, at each country's own retailers and on eBay, for the hardware that runs open AI models: graphics cards new and used, Macs and mini PCs, in the UK (GBP), Germany (EUR), Canada (CAD) and the US (USD). For any model: which of them run it fully in memory, how much memory it needs, and an estimated speed. Free and read-only, with no sign-up, account or API key.

This repository holds no server code. It has the connection settings for MCP clients, the registry entry (`server.json`) and an Agent Skill. The server runs at readygpu.com.

## Endpoint

```
https://readygpu.com/api/mcp
```

- Transport: Streamable HTTP, stateless. POST only: there is no SSE stream and no session.
- Authentication: none. No key, token or header.
- Tools: 6, all read-only.

## Tools

| Tool                 | What it does |
| -------------------- | ------------ |
| `search_models`      | Find open-weight AI models by name or task (chat, coding, agentic, vision, reasoning). Returns each model's slug, size, context length and licence; use the slug with hardware_for_model. |
| `find_hardware`      | List graphics cards or complete machines (Macs, mini PCs) priced in a market, with memory, bandwidth, today's best price and the estimated speed on a model. Filter by maximum price and minimum memory; sort by price, speed, value or price per GB. |
| `hardware_details`   | One graphics card or machine in a market: its specifications, every current price (new and used), and which models it runs fully in memory at a context length, how fast. |
| `hardware_for_model` | For one model: how much memory it needs at each context length, the cheapest, best-value and fastest hardware that runs it fully in memory in a market, and their prices and speeds. |
| `best_hardware`      | The ranked 'best for local AI' lists of a market (all graphics cards, cards under a price, used cards, Macs and mini PCs): best first by the most capable model each runs fully in memory, with prices. |
| `compare_hardware`   | Two graphics cards or machines side by side in a market: memory, price, how many models each runs fully in memory, and the estimated speed of each on every model. |

Markets: `us` (USD), `uk` (GBP), `de` (EUR), `ca` (CAD). Prices are each market's own retailers in its own currency, never converted. Speeds are estimates from memory bandwidth, not benchmarks.

## Connect

Setup steps for more clients, kept up to date: [readygpu.com/us/mcp](https://readygpu.com/us/mcp).

### Claude Code

```shell
claude mcp add --transport http readygpu https://readygpu.com/api/mcp
```

That adds it to the current project. Add `--scope user` to have it in every project. Check it with `claude mcp list`, or `/mcp` inside Claude Code.

This repository is also a Claude Code plugin (`.claude-plugin/plugin.json`, with `.mcp.json` and the skill in `skills/`).

### Claude (claude.ai and Claude Desktop)

Works on Free, Pro, Max, Team and Enterprise plans. Free allows one custom connector. On Team and Enterprise, an owner adds it for the organisation first.

1. Open **Customize › Connectors**.
2. Click **+ Add**, then **Add custom connector**.
3. Name it `readygpu`, paste `https://readygpu.com/api/mcp` as the remote MCP server URL and click **Continue**.
4. Review the detected settings and click **Continue**. Under **Authentication**, choose **No sign in**, then click **Add**.
5. In a chat, click **+** at the lower left, open **Connectors** and switch readygpu on.

On Team and Enterprise plans an owner adds it under **Organization settings › Connectors** (**Add**, then **Custom › Web**). Members then find it under **Customize › Connectors** and click **Connect**.

### ChatGPT

OpenAI offers custom MCP servers on ChatGPT Business, Enterprise and Edu, on the web only (not the mobile apps). On Business only admins and owners can add one; on Enterprise and Edu, admins can allow other members.

1. Go to [chatgpt.com/plugins](https://chatgpt.com/plugins), click the **+** button and choose **Add custom MCP server**.
2. Name it `readygpu`, enter `https://readygpu.com/api/mcp` as the **Server URL** and choose **No authentication**.
3. Review the warning, tick **I understand and want to continue**, then click **Create as a plugin**.
4. Open the new plugin in your plugins and click **+** to install it.
5. In a new chat, type `@` in the prompt box and pick readygpu.

### Cursor

Add this to `~/.cursor/mcp.json` to have it in every project, or to `.cursor/mcp.json` in one project:

```json
{
  "mcpServers": {
    "readygpu": {
      "url": "https://readygpu.com/api/mcp"
    }
  }
}
```

### VS Code (GitHub Copilot)

Works on every Copilot plan, Free included. With Copilot Business or Enterprise, your organisation's **MCP servers in Copilot** policy must be on; it is off by default.

1. Open the Command Palette and run **MCP: Add Server**.
2. Choose **HTTP**, paste `https://readygpu.com/api/mcp`, and enter `readygpu` as the server ID.
3. When asked where to save it, choose **Copilot Global** to have it in every workspace.
4. In Copilot Chat, click **Configure Tools** to check readygpu's tools are on. If VS Code asks whether you trust the server, confirm.

Or add it to `~/.copilot/mcp-config.json` yourself:

```json
{
  "mcpServers": {
    "readygpu": {
      "type": "http",
      "url": "https://readygpu.com/api/mcp"
    }
  }
}
```

In `.vscode/mcp.json`, or the profile's `mcp.json` (**MCP: Open User Configuration**), the top-level key is `servers` instead of `mcpServers`.

### OpenAI Codex

```shell
codex mcp add readygpu --url https://readygpu.com/api/mcp
```

Or add it to `~/.codex/config.toml`:

```toml
[mcp_servers.readygpu]
url = "https://readygpu.com/api/mcp"
```

The Codex CLI, the Codex IDE extension and the ChatGPT desktop app share this configuration. Check it with `codex mcp list`, or `/mcp` in the Codex terminal UI.

### Gemini CLI

```shell
gemini mcp add --transport http --scope user readygpu https://readygpu.com/api/mcp
```

Without `--scope user` it goes in the current project's `.gemini/settings.json`. To edit the file yourself, use `httpUrl`: in Gemini CLI, `url` means an SSE server.

```json
{
  "mcpServers": {
    "readygpu": {
      "httpUrl": "https://readygpu.com/api/mcp"
    }
  }
}
```

Check it with `/mcp` inside Gemini CLI. This repository is also a Gemini CLI extension (`gemini-extension.json`).

### Any other MCP client

- Server URL: `https://readygpu.com/api/mcp`
- Transport: Streamable HTTP (often labelled **HTTP** or `streamable-http`). There is no SSE stream and no session.
- Authentication: none.

A client that can only start local (stdio) servers, or only speaks the older SSE transport, cannot connect to it directly.

## Files

| File | For |
| ---- | --- |
| `server.json` | The MCP Registry entry (`com.readygpu/readygpu`, remote only) |
| `.mcp.json` | Claude Code (project config and plugin) |
| `mcp.json` | Cursor and other clients that read `mcpServers` with a `url` |
| `gemini-extension.json` | Gemini CLI extension |
| `.claude-plugin/plugin.json` | Claude Code plugin manifest |
| `skills/readygpu-local-ai-hardware/SKILL.md` | Agent Skill: when to use readygpu and how to quote its answers (also served at [readygpu.com](https://readygpu.com/.well-known/agent-skills/readygpu-local-ai-hardware/SKILL.md)) |
| `llms-install.md` | Install instructions for AI agents such as Cline |

## License

[MIT](LICENSE), for the files in this repository.
