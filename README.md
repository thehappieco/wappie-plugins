# Wappie plugins

The official Wappie plugin for **Codex** and **Claude Code**. It connects the
assistant to Wappie’s MCP connector at `https://mcp.wappie.thehappie.co/mcp`
and teaches it how to use it.

The connector is Wappie’s **attested reader**: a published image running in an
AWS Nitro Enclave, which your browser checks before it seals anything to it.
Message text, names and filenames are stored encrypted, and the key that opens
them for a connection is generated inside the enclave and never leaves it.

By default the assistant sees **metadata**: who, when, how much and what type
of message, not the content. When you connect, you choose in the Wappie console
which numbers it may read and, where your workspace allows them, what more it
may read, after confirming it is you:

- **message text and names**, opened inside the enclave;
- **attachments** (photos, stickers, PDFs, documents and a video’s preview
  image), opened inside the enclave, and AI transcripts of voice notes, audio
  and video where you set up an AI integration with your own provider key.

**Codex and Claude Code connect as apps on your computer**, and Wappie treats
them as such (reader 0.6.0):

- no **drafts** and no **notes to your own chat**: those are offered only to
  Claude and ChatGPT on the web, and nothing an assistant drafts reaches anyone
  until you send it yourself from the Wappie console;
- a connection with **message text lasts 7 days** unless you choose 1 or 30
  on the consent page (one with metadata only, 30 days unless you choose 7 or
  90);
- after **7 days without use** the app asks you to sign in again.

The assistant cannot send, delete or edit your messages and never marks a
message as read. You can revoke the connection in the console at any time;
revoking stops future reads, and cannot erase what the assistant already
received.

> **You do not need this plugin to connect.** Every assistant takes the
> connector’s address as it is: paste `https://mcp.wappie.thehappie.co/mcp` in
> Claude (or use the **Connect to Claude** button in the Wappie console), in
> Codex under *Settings → MCP servers*, in Claude Code with
> `claude mcp add --transport http --scope user wappie https://mcp.wappie.thehappie.co/mcp`,
> or as a plugin in ChatGPT’s developer mode (below). Install the plugin only if
> you want its skill, which tells the assistant what the connection can see, how
> to handle refusals, renewals and an ended connection, and not to read WhatsApp
> through the screen.

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

**ChatGPT on the web** (Plus, Pro, Business, Enterprise or Edu): Wappie is a
plugin you create yourself, on the web.

1. **Settings → Security and login → Developer mode**: turn it on.
2. Open <https://chatgpt.com/plugins> and choose **+**.
3. Name it `Wappie` and **upload the icon now**, in this same step:
   `plugins/wappie/assets/logo.png` from this repository (the same file the
   Wappie console’s ChatGPT tab offers). ChatGPT does not let you add or change
   it after **Create**.
4. Under **Connection**, a public endpoint: `https://mcp.wappie.thehappie.co/mcp`,
   authentication **OAuth**. Then **Create**, and sign in to the Wappie console
   when it opens.

Wappie then works in Chat and in Work (type `@` or use **+**). For attachments,
pick a model with reasoning (Thinking or Pro): Instant does not see images.

### How Codex and Claude Code sign in

Codex and Claude Code connect through their own sign-in: OAuth with a loopback
redirect (RFC 8252). The app identifies itself with a client metadata document
on its publisher’s site, opens the Wappie console in your browser, where you
sign in, choose your numbers and approve, and then receives the authorization
code on a local port of your own computer. The plugin holds no key, token or
password: only the connector’s address.

Connecting again from the same app replaces your previous connection of it in
that workspace, unless you untick **Replace my previous Codex connection**
(or Claude Code) on the consent page. When a connection has expired, was revoked, or lapsed after 7
days without use, the app reports that Wappie needs you to sign in again
(Claude Code: `/mcp`); signing in makes a new connection.

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
