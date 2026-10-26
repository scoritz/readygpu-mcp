# Installing the readygpu MCP server

These notes are for AI agents, such as Cline, that install MCP servers for a user.

readygpu is a remote MCP server. There is nothing to download, build or run, and no API key, account or environment variable to set.

- Server URL: `https://readygpu.com/api/mcp`
- Transport: Streamable HTTP, stateless (POST only; no SSE stream, no session)
- Authentication: none
- Tools (all read-only): `search_models`, `find_hardware`, `hardware_details`, `hardware_for_model`, `best_hardware`, `compare_hardware`

## Cline

Add this entry under `mcpServers` in `cline_mcp_settings.json` (in Cline, open **MCP Servers**, then **Configure MCP Servers**):

```json
{
  "mcpServers": {
    "readygpu": {
      "type": "streamableHttp",
      "url": "https://readygpu.com/api/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

Keep any servers already in the file.

## Check it works

Ask the server to list its tools; it should return the six above. A test call: `search_models` with `{ "query": "qwen", "limit": 2 }` returns two models with their slugs.

## Using it

Start with `search_models` or `find_hardware` to get slugs, then `hardware_for_model`, `hardware_details`, `compare_hardware` or `best_hardware`. Markets are `us` (USD), `uk` (GBP), `de` (EUR) and `ca` (CAD). Prices are in the market's own currency and change through the day; speeds are estimates, so say so when you quote them, and link the page each answer gives.

Other clients: see the [README](README.md) or [readygpu.com/us/mcp](https://readygpu.com/us/mcp).
