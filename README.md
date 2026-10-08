# Wappie plugins

The official Wappie plugin for **Claude Code** and **Codex**. It connects the
assistant to Wappie’s MCP connector at `https://mcp.wappie.thehappie.co/mcp`
and teaches it how to use it. Wappie has tested Claude Code; Codex connects as
an assistant Wappie has not tested, with that tier’s limits (below).

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

You confirm it is you in the Wappie console: nothing more if you signed in
within the last ten minutes, otherwise with your passkey or your password, or,
when your account signs in through The Happie Co, at The Happie Co in a window
the console opens. Wappie never asks for your password in an e-mail: its
notice of a new connection has one button, which only revokes that connection,
and its renewal notice has no link. To renew or to see your assistants, open
the Wappie console yourself.

## What each assistant gets (reader 0.6.0)

**Claude**, tested by Wappie on the web (claude.ai; Desktop and mobile use
the same connector). It is the only
assistant that may be offered **drafts** and **notes to your own chat**, where
your workspace allows them; nothing an assistant drafts reaches anyone until
you send it yourself from the Wappie console. Claude needs no plugin (below).

**Claude Code**, tested by Wappie as an **app on your computer**:

- no drafts and no notes to your own chat;
- a connection with **message text lasts 7 days** unless you choose 1 or 30
  on the consent page (one with metadata only, 30 days unless you choose 7 or
  90);
- after **7 days without use** it asks you to sign in again.

**Codex, ChatGPT and any other MCP client are not tested by Wappie.** ChatGPT
and Codex did not finish Wappie’s live tests, and Wappie gives no steps for
ChatGPT. An app that adds the address and identifies itself with its own
client metadata document connects as an **untested assistant**:

- the consent card is amber and names the app by its verified domain (for
  Codex and ChatGPT, `chatgpt.com`);
- message text and attachments only after a second, deliberate tick, and only
  if your e-mail address is confirmed;
- never drafts, notes to your own chat or sending;
- only the **last 7, 30 or 90 days** of history, as you choose (30 by default);
- at most **2,000 messages and 50 attachments a day** (300 and 10 in its first
  hour) and 20 calls a minute; at a limit the assistant is told when it
  resets (`limit_reached`), and an older message is refused as outside the
  window (`outside_window`);
- a connection with message text lasts 7 days unless you choose 1 or 30 (one
  with metadata only, 30 days unless you choose 7 or 90);
- after **3 days without use** when it reads text (7 with metadata only) it
  asks you to sign in again;
- a workspace holds at most three untested assistants and console connection
  tokens, among its ten connections.

No assistant sends a message to anyone else, deletes or edits your messages, or
marks a message as read. You can revoke a connection in the console at any
time; revoking stops future reads, and cannot erase what the assistant already
received.

> **You do not need this plugin to connect.** Every assistant takes the
> connector’s address as it is: paste `https://mcp.wappie.thehappie.co/mcp` in
> Claude (or use the **Connect to Claude** button in the Wappie console), in
> Claude Code with
> `claude mcp add --transport http --scope user wappie https://mcp.wappie.thehappie.co/mcp`,
> or in Codex as a remote MCP server (Wappie has not checked Codex’s current
> steps). Install the plugin only if you
> want its skill, which tells the assistant what the connection can see, how to
> handle refusals, limits, renewals and an ended connection, and not to read
> WhatsApp through the screen.

## Install the plugin

**Claude Code**:

```text
/plugin marketplace add thehappieco/wappie-plugins
/plugin install wappie@wappie
```

then run `/mcp` to sign in.

**Codex** (CLI), not tested by Wappie:

```bash
codex plugin marketplace add thehappieco/wappie-plugins
codex plugin add wappie@wappie
```

Start a new session; the first use opens the Wappie console to sign in and
choose your numbers, on the untested assistant’s card.

**Claude** (claude.ai, Desktop, mobile) does not need a plugin: use the
**Connect to Claude** button in the Wappie console, or add
`https://mcp.wappie.thehappie.co/mcp` as a custom connector.

### How Claude Code and Codex sign in

Claude Code and Codex connect through their own sign-in: OAuth with a loopback
redirect (RFC 8252). The app identifies itself with a client metadata document
on its publisher’s site, opens the Wappie console in your browser, where you
sign in, choose your numbers and approve, and then receives the authorization
code on a local port of your own computer. The plugin holds no key, token or
password: only the connector’s address.

Connecting again from the same app replaces your previous connection of it in
that workspace, unless you untick **Replace my previous … connection** on the
consent page (it names Claude Code, or an untested app by its domain).
A connection made before reader 0.6.0 is not replaced this way and keeps
what it was given until it ends: revoke it in the Wappie console.

When a connection has expired, was revoked, or lapsed without use, every call
fails with an authorization error and the app reports that Wappie needs you to
sign in again (Claude Code: `/mcp`); signing in makes a new connection. A
connection whose key the reader no longer holds has not ended: it keeps
reading metadata, and the assistant gives you a renewal link to the Wappie
console, where you confirm it is you.

## For a whole team

*Claude Team / Enterprise*: an owner adds the connector once in
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
