# Installing the betterimage.io MCP server

This is a hosted (remote) MCP server. There is nothing to clone, build or run locally, and no API key is needed for the free tools.

## Cline

Add this to `cline_mcp_settings.json` (Cline > MCP Servers > Configure):

```json
{
  "mcpServers": {
    "betterimage": {
      "type": "streamableHttp",
      "url": "https://betterimage.io/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

That is the whole setup. Verify by asking Cline to "make an OG image for https://betterimage.io/mcp"; the `card_from_url` tool should return an image.

## Optional: account tools

`list_templates`, `save_template`, `render_card`, `og_image_url` and `get_usage` need a betterimage.io account. Create an API key at https://betterimage.io/users/settings/api and add it as a header:

```json
{
  "mcpServers": {
    "betterimage": {
      "type": "streamableHttp",
      "url": "https://betterimage.io/mcp",
      "headers": {
        "Authorization": "Bearer <your betterimage.io API key>"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

Clients that support OAuth can use `https://betterimage.io/mcp/account` instead of a key.

## Other clients

See README.md for Claude Code, claude.ai, Cursor, VS Code and ChatGPT.
