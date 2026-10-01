# Wappie plugins

The official Wappie plugin for **Codex** and **Claude Code**. It connects the
assistant to Wappie's MCP connector at `https://mcp.wappie.thehappie.co/mcp`
and teaches it how to use it.

The connector is Wappie's **attested reader**: a published image running in an
AWS Nitro Enclave, which your browser checks before it seals anything to it.
Message text, names and filenames are stored encrypted, and the key that opens
them for a connection is generated inside the enclave and never leaves it.

By default the assistant sees **metadata**: who, when, how much and what type
of message, not the content. When you connect, you choose in the Wappie console
which numbers it may read and, where Wappie has enabled them for your
workspace, what more it may do, each confirmed with your password:

- **message text and names**, opened inside the enclave;
- **attachments** (photos, stickers, PDFs, documents and a video's preview
  image), opened inside the enclave, and AI transcripts of voice notes, audio
  and video where you set up an AI integration with your own provider key;
- **drafts** that you review and confirm yourself in the Wappie console;
  nothing the assistant drafts reaches anyone until you do;
- **notes to your own chat**.

The tools that write a draft or a note exist only on a connection whose consent
allows them. The assistant cannot delete or edit your messages and never marks
a message as read. You can revoke the connection in the console at any time;
revoking stops future reads, and cannot erase what the assistant already
received.

> **You do not need this plugin to connect.** Every assistant takes the
> connector's address as it is: paste `https://mcp.wappie.thehappie.co/mcp` in
> Claude (or use the **Connect to Claude** button in the Wappie console), in
> Codex under *Settings → MCP servers*, in Claude Code with
> `claude mcp add --transport http --scope user wappie https://mcp.wappie.thehappie.co/mcp`,
> or in ChatGPT's developer mode. Install the plugin only if you want its skill,
> which tells the assistant what the connection can see, how to handle drafts,
> refusals and renewals, and not to read WhatsApp through the screen.

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

then run `/mcp` to sign in.

**Claude** (claude.ai, Desktop, mobile) does not need a plugin: use the
**Connect to Claude** button in the Wappie console, or add
`https://mcp.wappie.thehappie.co/mcp` as a custom connector.

**ChatGPT on the web**: turn on developer mode in settings and add
`https://mcp.wappie.thehappie.co/mcp` as a plugin. For attachments, pick a
model with reasoning (Thinking or Pro): Instant does not see images.

### How Codex and Claude Code sign in

Codex and Claude Code connect through their own sign-in: OAuth with a loopback
redirect (RFC 8252). The app identifies itself with a client metadata document
on its publisher's site, opens the Wappie console in your browser, where you
sign in, choose your numbers and approve, and then receives the authorization
code on a local port of your own computer. The plugin holds no key, token or
password: only the connector's address.

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
