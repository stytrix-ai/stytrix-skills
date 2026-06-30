# StyTrix Agent Skills

Agent skills for **[StyTrix](https://www.stytrix.com)** — design fashion with AI directly from your coding agent or assistant. The skill teaches your agent to act as a fashion design assistant by orchestrating the [StyTrix MCP server](https://www.stytrix.com/mcp): generate concepts, models, fabrics, sketches, mix-and-match looks, and runway videos, and place them live on a StyTrix canvas.

## Install

Open your favorite agent (Claude Code, Codex, Cursor, etc.) and run:

```bash
npx skills add https://github.com/hirosichen/stytrix-skills --skill stytrix
```

This installs the [`stytrix`](skills/stytrix/SKILL.md) skill into your agent's skills directory. Works with Claude Code, Cursor, Codex, Continue, and other agents that support the open Agent Skills format.

## Connect the StyTrix MCP server

The skill drives the StyTrix MCP tools, so connect the server once:

- **Claude.ai:** Settings → Connectors → Add custom connector → `https://www.stytrix.com/api/mcp`
- **Claude Code:** `claude mcp add --transport http stytrix https://www.stytrix.com/api/mcp`
- **Cursor / Codex / Gemini:** add `https://www.stytrix.com/api/mcp` to your MCP config

Auth is OAuth 2.1 (no API key). Read-only tools are free; generation tools use StyTrix credits. Full docs: https://www.stytrix.com/mcp

## What you can do

Once connected, just ask your agent things like:

- "Use StyTrix to generate a photorealistic oversized wool coat concept and put it on my canvas."
- "Build an SS26 linen capsule: a model, three fabric swatches, and two looks."
- "Turn this jacket photo into a clean technical flat sketch."

## Skills in this repo

| Skill | Install | What it does |
|-------|---------|--------------|
| [`stytrix`](skills/stytrix/SKILL.md) | `npx skills add https://github.com/hirosichen/stytrix-skills --skill stytrix` | AI fashion design via the StyTrix MCP |

## Links

- Website: https://www.stytrix.com
- MCP docs: https://www.stytrix.com/mcp
- MCP server repo: https://github.com/hirosichen/stytrix-mcp
- CLI: https://github.com/hirosichen/stytrix-cli (`npx stytrix`)
- Support: hello@stytrix.com
