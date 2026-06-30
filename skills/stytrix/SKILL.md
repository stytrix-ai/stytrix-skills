---
name: stytrix
description: Design fashion with AI using StyTrix. Use when the user wants to generate or iterate on fashion designs — garment concepts, fashion models / lookbooks, fabric and textile swatches, technical flat sketches, mix-and-match outfits, multi-angle product views, upscales, background removal, runway videos, or a custom-trained style — and place the results live on a StyTrix design canvas. Triggers on requests to "design", "generate", or "create" garments / apparel / textiles / lookbooks; building a collection or capsule; turning a brief, moodboard, or reference image into fashion concepts; producing tech-pack flats; or checking StyTrix projects and credits. Works through the StyTrix MCP server (https://www.stytrix.com/api/mcp).
compatibility: Requires the StyTrix MCP server to be connected in the agent (Claude.ai, Claude Code, Cursor, Codex, or Gemini) and a StyTrix account with credits for generation tools.
metadata:
  author: StyTrix
  source: https://github.com/hirosichen/stytrix-skills
allowed-tools: Bash(npx -y stytrix *)
user-invocable: true
---

# StyTrix — AI Fashion Design

StyTrix is an AI fashion design platform. This skill makes you a fashion design assistant that orchestrates the StyTrix MCP tools: turn a brief into concepts, models, fabrics, and full looks, and place every result **live on the user's StyTrix canvas** — without leaving the chat.

## Prerequisites — make sure StyTrix is connected

The StyTrix tools come from the StyTrix MCP server. If you don't see tools like `list_projects` or `generate_concept`, the server isn't connected yet. Tell the user how to connect it:

- **Claude.ai:** Settings → Connectors → **Add custom connector** → URL `https://www.stytrix.com/api/mcp` → **Connect** (sign in with their StyTrix account).
- **Claude Code:** `claude mcp add --transport http stytrix https://www.stytrix.com/api/mcp`
- **Cursor / Codex / Gemini:** add the same URL to the MCP config.

Authentication is OAuth 2.1 (no API key). Read-only tools are free; generation tools spend StyTrix credits. Docs: https://www.stytrix.com/mcp

### Two ways to use StyTrix

- **Preferred — MCP tools.** If the StyTrix MCP is connected, call the tools directly (`whoami`, `generate_concept`, …). The rest of this skill assumes this path.
- **Fallback — the `stytrix` CLI.** If MCP tools aren't available (e.g. a terminal agent with no MCP connection), use the StyTrix CLI over Bash — it signs in with the same OAuth and exposes the same capabilities:
  - `npx -y stytrix login` (one-time browser sign-in)
  - `npx -y stytrix projects` / `credits` / `whoami`
  - `npx -y stytrix generate --project <id> --prompt "..." [--mode photorealistic|true_to_sketch] [--ref <imageUrl>]`
  - `npx -y stytrix call <tool> '<json-args>'` for any other tool

  The CLI maps 1:1 to the tools below; the same rules and workflows apply.

## The tools

**Read-only (free):** `whoami`, `list_projects`, `get_credits`, `check_video`, `check_style_training`.

**Write (spend credits; results land on a canvas):** `create_project`, `add_image_to_canvas`, `generate_concept` (mode: `photorealistic` | `true_to_sketch`), `mix_match`, `generate_style`, `generate_model`, `generate_fabric`, `upscale_image`, `remove_background`, `image_to_sketch`, `multi_angle`, `split_layer`, `start_video` (+ `check_video`), `start_style_training` (+ `check_style_training`).

## Core rules

1. **Orient first, for free.** Begin with `whoami`, `list_projects`, and `get_credits` so you know which account, which canvas, and the credit budget — before spending anything.
2. **Always target a project.** Every write tool needs a `projectId`. Use an existing project (from `list_projects`) or `create_project`. Results appear live at `https://www.stytrix.com/canvas/{projectId}` — always share that link.
3. **Confirm before spending credits.** Generation deducts credits. For batch or multi-step work, estimate the spend and get the user's go-ahead first.
4. **Charged only on success.** If a tool errors, no credits are lost — surface the message, then adjust and retry.
5. **Async tools poll.** `start_video` and `start_style_training` return an id; poll `check_video` / `check_style_training` until they report done, then share the result URL.
6. **One change at a time, then iterate.** Generate, show the canvas link, ask what to refine (colorway, fabric, silhouette), and regenerate — don't batch dozens of credits silently.

## Workflows

Common recipes — pick by intent. Full step lists and copy-ready prompts are in [references/workflows.md](references/workflows.md).

- **Brief → concept:** `list_projects` → (create/choose project) → `generate_concept` (photorealistic) from the user's description → share canvas link → iterate.
- **Edit a reference / sketch:** `generate_concept` with `mode: true_to_sketch` and `referenceImageUrl` to restyle an existing image or sketch.
- **Capsule collection:** create a project → `generate_model` → `generate_fabric` (×N) → `generate_style` / `mix_match` to assemble looks → review on one canvas.
- **Tech-pack flat:** `image_to_sketch` on a garment photo to get a clean black-and-white technical flat.
- **Product views:** `multi_angle` to get coordinated front / back / side views from one reference.
- **Finishing:** `upscale_image`, `remove_background`, `split_layer` to polish a chosen result.
- **Runway video:** `start_video` from a prompt or image, then `check_video` until ready.
- **Custom style:** `start_style_training` from reference images, then `check_style_training`.

## Output etiquette

- Describe what you generated in one line, then give the **canvas link** and the **credit cost / new balance** (the tools return these).
- If credits are insufficient, say so and point the user to top up — don't keep retrying.
- Respect the user's creative direction; offer options, not lectures.
