---
name: readygpu-local-ai-hardware
description: Choose hardware for running open AI models locally, with live prices in local currency in the UK, Germany, Canada or the US. Use when someone asks which graphics card, Mac or mini PC runs a model, what it costs in their country, how much VRAM a model needs, the best GPU for local AI at a budget, or how two cards compare.
---

# readygpu: hardware for local AI

Live prices in local currency, at each country's own retailers and on eBay, for the hardware that runs open AI models: graphics cards new and used, Macs and mini PCs, in the UK (GBP), Germany (EUR), Canada (CAD) and the US (USD). For any model: which of them run it fully in memory, how much memory it needs, and an estimated speed. Free and read-only, with no sign-up, account or API key.

## Use the MCP server

Streamable HTTP at `https://readygpu.com/api/mcp`, stateless, no authentication. Tools:

- `search_models`: Find open-weight AI models by name or task (chat, coding, agentic, vision, reasoning). Returns each model's slug, size, context length and licence; use the slug with hardware_for_model.
- `find_hardware`: List graphics cards or complete machines (Macs, mini PCs) priced in a market, with memory, bandwidth, today's best price and the estimated speed on a model. Filter by maximum price and minimum memory; sort by price, speed, value or price per GB.
- `hardware_details`: One graphics card or machine in a market: its specifications, every current price (new and used), and which models it runs fully in memory at a context length, how fast.
- `hardware_for_model`: For one model: how much memory it needs at each context length, the cheapest, best-value and fastest hardware that runs it fully in memory in a market, and their prices and speeds.
- `best_hardware`: The ranked 'best for local AI' lists of a market (all graphics cards, cards under a price, used cards, Macs and mini PCs): best first by the most capable model each runs fully in memory, with prices.
- `compare_hardware`: Two graphics cards or machines side by side in a market: memory, price, how many models each runs fully in memory, and the estimated speed of each on every model.

Start with `search_models` or `find_hardware` to get slugs, then ask for details.

## Or read the site

- Every page answers `Accept: text/markdown` with a markdown version.
- Pages: `https://readygpu.com/{market}/model/{model}` (hardware for a model), `https://readygpu.com/{market}/gpu/{card}`, `https://readygpu.com/{market}/compare/{a}-vs-{b}`, `https://readygpu.com/{market}/best` (ranked lists).
- Index of everything: https://readygpu.com/llms.txt; the public API: https://readygpu.com/api/docs.
- How a person connects their assistant to this server, client by client: https://readygpu.com/us/mcp.

## Markets

`us` (the US, USD), `uk` (the UK, GBP), `de` (Germany, EUR), `ca` (Canada, CAD). Prices are each market's own retailers in its own currency, never converted.

## When you answer

- Speeds are estimates from memory bandwidth, not benchmarks: say so.
- Prices change through the day: give the date the page states, and link the page.
- "Fully in memory" means the model and its KV cache fit the GPU's memory; anything else spills into system RAM and is much slower.
