# betterimage.io MCP server

Social cards and OG images for your AI assistant. Give Claude, Cursor, VS Code or any MCP client a page URL and get back a link-preview card on a design picked to fit the page; browse 100 starter designs, preview them with your own text, check how a link previews on X, LinkedIn, Slack and Discord, generate Open Graph and Twitter tags, and with an account save templates, render clean images and get signed og:image URLs that need no hosting.

Every image is rendered from a designed template by a deterministic renderer: the text changes, layout, fonts and colours stay on-brand. No generative image model is involved.

This repository holds the public distribution files for the hosted server (install manifests, registry metadata). The server itself runs at **https://betterimage.io/mcp** and is documented at https://betterimage.io/mcp.

## Endpoints

| URL | Auth | Use |
|---|---|---|
| `https://betterimage.io/mcp` | none (optional API key) | Free tools work with no account. Pass `Authorization: Bearer <api key>` to unlock the account tools. |
| `https://betterimage.io/mcp/account` | OAuth 2.1 | Same server; an unauthenticated call answers 401 and starts the sign-in flow in claude.ai, Claude Desktop, Claude Code and other OAuth-capable clients. |

Transport: Streamable HTTP. Protocol revisions 2024-11-05 through 2026-07-28.

## Install

**Claude Code**

```bash
# free tools, no account
claude mcp add --transport http betterimage https://betterimage.io/mcp

# your own templates: sign in once (run /mcp in a session, pick betterimage, Authenticate)
claude mcp add --scope user --transport http betterimage https://betterimage.io/mcp/account
```

**claude.ai / Claude Desktop**: Settings > Connectors > Add custom connector, URL `https://betterimage.io/mcp/account`.

**Cursor**: one-click install button on https://betterimage.io/mcp, or add to `mcp.json`:

```json
{
  "mcpServers": {
    "betterimage": { "url": "https://betterimage.io/mcp" }
  }
}
```

**VS Code**: one-click install button on https://betterimage.io/mcp, or add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "betterimage": { "type": "http", "url": "https://betterimage.io/mcp" }
  }
}
```

**Gemini CLI**: `gemini extensions install https://github.com/mfundakowski/betterimage-mcp`, or add to `settings.json`:

```json
{
  "mcpServers": {
    "betterimage": { "httpUrl": "https://betterimage.io/mcp" }
  }
}
```

**ChatGPT** (Plus/Pro, Developer mode): Settings > Apps & Connectors > Create, URL `https://betterimage.io/mcp`, no authentication.

**Any other client**: point it at `https://betterimage.io/mcp` over Streamable HTTP.

## Tools

No account needed:

- `card_from_url`: a social card for an existing page, in one call. Fetches the page like a crawler, picks the design that fits it, returns the image (watermarked preview).
- `list_presets`: the 100 starter designs with key, size, categories and preview image.
- `preview_card`: render a preset with your text (watermarked preview, no quota).
- `check_link_preview`: what X, Facebook, LinkedIn, Slack and Discord will show for a URL, with concrete fixes.
- `generate_meta_tags`: the complete `<head>` block with Open Graph and Twitter tags.

With an account (OAuth or API key):

- `list_templates`, `save_template`: your saved designs.
- `render_card`: a clean PNG from a saved template (paid plans; watermarked on the free tier).
- `og_image_url`: a signed og:image URL rendered on first fetch and cached at the edge, so every page gets its own card with no hosting.
- `get_usage`: plan, watermark status and monthly quota.

Every render tool's `fields` object also takes the card's pictures by URL -
`image_url`, `image2_url`, `logo_url` and `background_url` - fetched
server-side and drawn in the slot the design already lays out. That is what
turns one saved template into a whole catalogue: one card per product, same
colours, fonts and crop framing. PNG, JPEG or WebP up to 5 MB, decided by the
bytes rather than the URL; the fetch is SSRF-hardened and a URL pointing back
at betterimage.io is refused.

Every tool carries a title and `readOnlyHint`/`destructiveHint` annotations; the two write tools are `save_template` and `render_card`.

## Registries

- Official MCP Registry: `io.betterimage/betterimage` (`server.json` in this repo)
- Smithery: `@matfun93/betterimage`

## Cursor plugin

`.cursor-plugin/plugin.json` and `mcp.json` make this repository installable as a Cursor plugin; the plugin only registers the hosted server.

## In JavaScript

For code rather than an assistant, [`@betterimage/cards`](https://www.npmjs.com/package/@betterimage/cards) builds the same signed image URLs from Node, Bun, Deno or an edge runtime, with no dependencies.

## Links

- Documentation: https://betterimage.io/mcp
- Privacy: https://betterimage.io/privacy
- Terms: https://betterimage.io/terms
- Support: hello@betterimage.io
