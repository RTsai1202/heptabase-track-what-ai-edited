# Heptabase Track Changes

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

## Install

Tell your agent (with Heptabase connected through MCP or the CLI):

> Install the Heptabase Skill from https://github.com/RTsai1202/heptabase-track-changes by following its INSTALL.md.

Or create it manually in Heptabase: make a new Skill with the command, display name, and description in [`skill/skill.json`](skill/skill.json), and paste [`skill/instructions.md`](skill/instructions.md) as its instructions.

Optional: agents with their own skill folders can also install [`agent-skill/heptabase-track-changes`](agent-skill/heptabase-track-changes/SKILL.md), a short pointer that makes them load the Heptabase Skill whenever they edit Heptabase content.

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
