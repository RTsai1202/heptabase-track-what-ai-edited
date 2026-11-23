# Heptabase Track What AI Edited

[繁體中文](README.zh-TW.md)

A Heptabase Skill that makes edits by external AI agents reviewable.

Heptabase's built-in AI shows its edits as a red and green diff before you accept them. Agents that edit through the Heptabase MCP server or CLI write changes directly, so you cannot see what changed. This Skill makes those agents mark every change in place:

| Change | How it looks |
| --- | --- |
| Deleted text | Red text with strikethrough |
| Added text | Green text |
| Each changed region | A to-do above it: `🤖 AI edit E1 · 2026-11-23 14:05 · Claude Code · Shortened the conclusion` |

After editing, the agent shows the changed cards right in the chat, with the colors and markers visible, in clients that render MCP Apps (Claude, Claude Code desktop, ChatGPT). Text-only clients get a link to each card instead.

The to-do markers make changes easy to spot and jump to from a whiteboard. When you are done reviewing, check the markers you accept and tell the agent to resolve. It removes the marks: accepted changes become plain text, rejected ones revert.

## Why a Heptabase Skill

Heptabase stores Skills as instruction cards in your own space. Every connected agent (Claude Code, Codex, Claude, ChatGPT, Grok, and others) reads the same card, so you install once and maintain one copy.

The Heptabase AI Agent inside the app does not need this Skill: its edits already go through Heptabase's built-in review. The card tells it to edit the built-in way. The one exception is cleaning up markers that an external agent left, which it may do on request.

Heptabase's own diff view lives in the app and is not saved in card content, and the "Created by AI" flag only covers newly created cards. Colored text and to-do items are ordinary content, so they work with every agent and survive sync.

## Which agents can use it

Any agent connected to Heptabase through MCP or the CLI can run Skills saved in your space. Heptabase shipped this in MCP & Heptabase CLI 0.11.0.

| Source | What it says |
| --- | --- |
| [Heptabase public roadmap](https://wiki.heptabase.com/roadmap/) | "MCP & Heptabase CLI 0.11.0: Use skills such as Break down book, and create and update your own skills." Shipped on 2026-11-10 (v1.117.1). |
| [How to use Heptabase MCP?](https://support.heptabase.com/en/articles/12679581-how-to-use-heptabase-mcp) | Lists "Use skills: run the skills saved in your space" among MCP capabilities. |
| Heptabase system manual, `skills` page (served by the MCP server and CLI) | A Skill lives in your knowledge base, so the Heptabase AI Agent and every connected agent use the same one. |

Clients listed in the official MCP guide: ChatGPT (custom MCP app on the web, or the official plugin), Claude and Codex (official plugin), Claude Code, Cursor, Antigravity CLI, Grok (Grok Bot or a custom connector on grok.com), and other services that support remote MCP servers.

How each kind of agent finds this Skill:

- **Web chat apps** (ChatGPT, Claude, Grok): through MCP. Agents see the Skill list when they read your Space Instructions. If an agent skips it, say "use track-what-ai-edited" or "mark the changes".
- **Agents with skill folders** (Claude Code, Codex, Antigravity, OpenCode, and others that read `SKILL.md`): through MCP or the CLI, plus the optional [pointer skill](agent-skill/heptabase-track-what-ai-edited/SKILL.md), which makes them load this Skill whenever they edit Heptabase content.

## Install

Tell your agent (with Heptabase connected through MCP or the CLI):

> Install the Heptabase Skill from https://github.com/RTsai1202/heptabase-track-what-ai-edited by following its INSTALL.md.

Or create it manually in Heptabase: make a new Skill with the command, display name, and description in [`skill/skill.json`](skill/skill.json), and paste [`skill/instructions.md`](skill/instructions.md) as its instructions.

Optional: agents with their own skill folders can also install [`agent-skill/heptabase-track-what-ai-edited`](agent-skill/heptabase-track-what-ai-edited/SKILL.md), a short pointer that makes them load the Heptabase Skill whenever they edit Heptabase content.

## Use

- "Rewrite the second paragraph of this card." The agent asks once whether to mark changes or edit directly, then marks them.
- "Resolve the AI edits on this card." Checked markers are accepted; unchecked ones stay for later.
- "Accept E1, reject E2." / "Accept all."

## Update

After installing, the instruction card in your Heptabase space is your working copy. To change the Skill, ask any connected agent to edit that card; every agent uses the new version on its next run. Changing this repository does not change your installed copy. To pull a newer release from this repository, run the install again: the agent compares versions and asks before replacing your card.

## Limits

- Code blocks and display math cannot be colored. The agent keeps the old block and adds the new one below it.
- In mark mode the agent never deletes images, embeds, or complex tables; it describes the proposal in the marker instead.
- Resolve treats red strikethrough and green text as tracked changes only inside a marked region, so your own colored text elsewhere is safe.

## License

MIT
