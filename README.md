# Wappie plugins

The official Wappie plugin for **Codex** and **Claude Code**. It connects the
assistant to Wappie's hosted MCP connector at
`https://api.wappie.thehappie.co/mcp` and teaches it how to use it.

The connector is **read-only and metadata-only**: the assistant sees who, when,
how much and what type of message, never the content. Message text, names and
filenames are stored encrypted, and the hosted connector holds no key to open
them. You choose which numbers it may read
when you sign in to the Wappie console, and you can revoke the connection there
at any time.

> **You do not need this plugin to connect.** Every assistant takes the
> connector's address as it is: paste `https://api.wappie.thehappie.co/mcp` in
> Claude (or use the **Connect to Claude** button in the Wappie console), in
> Codex under *Settings → MCP servers*, in Claude Code with
> `claude mcp add --transport http --scope user wappie https://api.wappie.thehappie.co/mcp`,
> or in ChatGPT's developer mode. Install the plugin only if you want its skill,
> which tells the assistant what the connection can see and not to read
> WhatsApp through the screen.

## Install the plugin

**Codex** (ChatGPT desktop app or CLI):

```bash
codex plugin marketplace add thehappieco/wappie-plugins
codex plugin add wappie@wappie
```

or, in the desktop app, open **Plugins**, pick the **Wappie** source and install.
Start a new session; the first use opens the Wappie console to sign in and
choose your numbers.

**Claude Code**:

```text
/plugin marketplace add thehappieco/wappie-plugins
/plugin install wappie@wappie
```

**Claude** (claude.ai, Desktop, mobile) does not need a plugin: use the
**Connect to Claude** button in the Wappie console, or add
`https://api.wappie.thehappie.co/mcp` as a custom connector.

**ChatGPT on the web**: turn on developer mode in settings and add
`https://api.wappie.thehappie.co/mcp` as a plugin.

## For a whole team

- *ChatGPT Business / Enterprise*: a workspace admin imports this repository in
  **Admin → Plugins → Import marketplace**; members then get Wappie from the
  workspace directory.
- *Claude Team / Enterprise*: an owner adds the connector once in
  **Organization settings → Connectors**; members click **Connect**.

## What is in here

| Path | For |
|---|---|
| `.agents/plugins/marketplace.json` | Codex marketplace |
| `.claude-plugin/marketplace.json` | Claude Code marketplace |
| `plugins/wappie/.codex-plugin/plugin.json`, `mcp.json` | Codex plugin and its MCP server |
| `plugins/wappie/.claude-plugin/plugin.json`, `.mcp.json` | Claude Code plugin and its MCP server |
| `plugins/wappie/skills/wappie/SKILL.md` | how the assistant should use Wappie |

Privacy: <https://thehappie.co/en/privacy/> · Terms: <https://thehappie.co/en/terms/> ·
Product: <https://wappie.thehappie.co/>

Licensed under the Apache License 2.0.
