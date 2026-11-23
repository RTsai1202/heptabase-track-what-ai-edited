# Install guide (for AI agents)

This file is written for the AI agent that installs the Skill. A person can hand it over with one sentence:

> Install the Heptabase Skill from https://github.com/RTsai1202/heptabase-track-changes by following its INSTALL.md.

The Skill lives in the user's Heptabase main space as a Heptabase Skill. Every agent connected through the Heptabase MCP server or CLI then reads the same instruction card, so there is only one copy to maintain. After installation, that card is the user's working copy: later changes are made by editing the card, not this repository. The built-in Heptabase AI Agent is told by the card itself to keep using its own review flow.

## Requirements

One of:

- The Heptabase MCP server, with Skill tools available.
- The Heptabase CLI (`heptabase skill --help` lists `create`), with the desktop app running.

If neither is available, stop and tell the user to connect Heptabase first.

## Steps

1. Read the Heptabase system manual page `skills` once.
2. Read `skill/skill.json` and `skill/instructions.md` from this repository. Use `instructions.md` verbatim as the instructions; do not add a title, because Heptabase adds `# Skill: <displayName>`.
3. List the Skills in the user's space.
   - If `/track-changes` does not exist: show the user the display name, command, description, and a one-line example from `skill.json`, then create the Skill after they confirm.
   - If `/track-changes` already exists: read its instruction card, tell the user how it differs from this version, and ask whether to update it. To update, replace the instruction card's content below its title line with `instructions.md`, and update the description if it changed. Never create a second copy under another name.
   - If the command name is taken by an unrelated Skill: ask the user for another command name.
4. Report the command and a link to the instruction card. Tell the user that to change the Skill later, they ask an agent to edit this card.

## Optional: host skill pointer

Agents with their own skill system (Claude Code, Codex, and others that read `SKILL.md`) may not open Heptabase Skills on their own when they edit a card. Install `agent-skill/heptabase-track-changes/` into the host's skills folder so the host loads the Heptabase Skill every time it edits Heptabase content. The pointer holds no rules of its own, so the Heptabase instruction card stays the single source.

## Uninstall

Move the instruction card to trash. The command disappears with it; restoring the card brings it back.
