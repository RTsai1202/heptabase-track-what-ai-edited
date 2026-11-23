## Purpose

When an agent changes existing text in a card or journal, the user should see exactly what changed, review it from the whiteboard, and accept or reject it. This Skill marks each change in place, like tracked changes in a word processor, and later cleans the marks up.

It has two modes:

- **Mark**: make the requested change as a visible suggestion.
- **Resolve**: accept or reject marked suggestions, then remove all marks.

Write the marker text, summaries, and replies in the user's language. Keep the fixed prefix `🤖 AI edit` in every language, because Resolve finds markers by it.

## Choosing the mode

- The user asks to change, rewrite, fix, translate, shorten, or restructure existing content: use Mark.
- The user says accept, reject, approve, resolve, clean up, or checks off markers and asks to finish: use Resolve.
- New content added to an empty card, or a brand-new card: no marks needed.
- Before the first Mark edit in a request, if the user has not said how to apply changes, ask once. Use a structured question if your host has one. Offer two choices: **Mark changes for review** (recommended) and **Edit directly**. Do not ask again within the same request. If the user says "edit directly", "just replace", or "mark it", follow that without asking.
- Edit directly means a normal edit with no marks and no marker. Say in the reply what changed.

## Mark mode

### 1. Read and plan

Read the latest content of the card or journal. Change only what the request covers. Group the changes into **hunks**: one hunk is one contiguous region of changed blocks. Two changes separated by unchanged blocks are two hunks.

### 2. Write each change

Deleted text: red text with strikethrough.

`<hepta-color type="text" color="red">~~old text~~</hepta-color>`

Added text: green text.

`<hepta-color type="text" color="green">new text</hepta-color>`

Choose the granularity by the size of the change:

- **Word or phrase inside a sentence**: put the deleted part and the added part side by side, deleted first, at the exact spot, as in the first line of the example at the end.
- **Pure insertion**: green only. **Pure deletion**: red strikethrough only.
- **Most of a sentence or more**: keep the whole old block as red strikethrough, then put the whole new block in green directly below it. Do not interleave many small fragments; a reader must be able to read the old version and the new version in one pass each.
- **Several blocks**: all old blocks in red strikethrough first, then all new blocks in green.
- **Headings and list items**: same rules; keep the block type and indentation.

Content that cannot be colored:

- **Code blocks and display math**: keep the old block, add the new block directly below it, and say "old block above, new block below" in the marker.
- **Images, embeds, files, and protected content**: never delete them in Mark mode. Leave them in place and describe the proposed removal or move in the marker.
- **Complex tables**: do not edit them in Mark mode. Describe the proposal in the marker. Cells of simple tables follow the inline rules.

Never change text outside the hunks. Keep mentions, links, citations, and protected content exactly as they were inside both the old and the new text.

### 3. Add one marker per hunk

Insert a to-do item directly above the first block of each hunk, at the same indentation level as that block:

`- [ ] 🤖 AI edit E<n> · <YYYY-MM-DD HH:MM> · <editor> · <summary>`

- **E<n>**: a number unique within this card or journal. Use the next number after the highest `E` number already present, starting at E1.
- **Date and time**: the user's local time.
- **Editor**: the agent or tool making the edit, such as `Claude Code`, `Codex`, or `Heptabase AI`; add the model name when known. If a person asked for it on someone else's behalf, write `<agent> for <person>`.
- **Summary**: one short sentence on what changed and why, such as "Shortened the conclusion and fixed the date".

If one request produces more than 8 hunks in the same card, ask before writing whether to keep one marker per hunk or use one marker at the top of the card that lists all changes.

### 4. Write once and verify

Apply all hunks and markers of one card in a single edit, so the card is never left half-marked. Check the saved result: every hunk has its marker, the old text is intact inside the red marks, and nothing outside the hunks changed.

### 5. Reply

Give a link to each changed card or journal and list its markers with their summaries. Tell the user how to finish: check the markers they accept and ask to resolve, or name markers to accept or reject, such as "accept E1, reject E2".

## Resolve mode

### 1. Find the markers

Use the card or journal the user points at. If they point at none, search the knowledge base for `🤖 AI edit` and confirm the list with the user before changing anything. Read the latest content.

A marker's hunk is the run of blocks below it that contain red strikethrough or green text, up to the next marker or the first block without either. Treat red text with strikethrough as deleted text and green text as added text only inside a hunk. Never touch colors outside a hunk.

### 2. Decide each marker

- The user named the marker: do what they said.
- The user said "accept all" or "reject all": apply it to every marker in scope.
- No explicit decision: a checked marker means accept. Leave unchecked markers as they are and list them in the reply.

### 3. Apply

- **Accept**: delete the red strikethrough text. Turn green text into plain text by removing only the green color. Delete the marker.
- **Reject**: delete the green text. Turn red strikethrough text back into plain text by removing the red color and the strikethrough. Delete the marker.
- For a code block or display math pair, keep the new block on accept and the old block on reject.
- If the user has edited text inside a hunk, keep their wording and only remove the marks. When their edit makes the decision unclear, ask.
- Remove empty blocks left behind and keep the surrounding structure and indentation.

Apply all decisions for one card in a single edit, then check that no marks or markers remain for the resolved hunks and that unresolved hunks are untouched.

### 4. Reply

Report per card: which markers were accepted, rejected, or left open, with a link to the card.

## Example

Request: "Change the meeting to Thursday and make the closing paragraph shorter."

Result in the card:

```
- [ ] 🤖 AI edit E1 · 2026-11-23 14:05 · Claude Code (Opus 5.5) · Moved the meeting to Thursday

The meeting is on <hepta-color type="text" color="red">~~Wednesday~~</hepta-color><hepta-color type="text" color="green">Thursday</hepta-color> afternoon.

- [ ] 🤖 AI edit E2 · 2026-11-23 14:05 · Claude Code (Opus 5.5) · Shortened the closing paragraph

<hepta-color type="text" color="red">~~In conclusion, after considering all of the points above in detail, we believe that the plan is feasible and should move forward.~~</hepta-color>

<hepta-color type="text" color="green">The plan is feasible and should move forward.</hepta-color>
```

Later the user checks E2, leaves E1 open, and says "resolve": the closing paragraph becomes the plain short version, E2 disappears, and E1 stays for review.
