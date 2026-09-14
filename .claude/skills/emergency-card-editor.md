---
name: emergency-card-editor
description: Safely edit the All-in-One Emergency Reference Card (全方位緊急應變小卡) — a single self-contained offline HTML file whose content is tripled across ZH/EN/Markdown copies. Use whenever adding, editing, or removing a topic, an image, or a feature in this project, or when asked to sync/verify/publish it.
---

# Editing the Emergency Reference Card

This is a single-file, zero-dependency, offline-first HTML tool (81 topics, ZH/EN toggle, dark/light theme). Read `CLAUDE.md` at the project root first for the structural facts (the 3-copy content pattern, the `index.html` duplicate, image-embedding rules). This skill is the step-by-step procedure for making a change safely.

## Step 1 — classify the change

- **Topic content** (a fact, a step list, a table row, a photo inside a card): touches up to 4 places — the initial `<main>` block, `ZH_MAIN_HTML`, `EN_MAIN_HTML`, and `MD_CONTENT` (skip `MD_CONTENT` only for things Markdown can't represent, like an SVG diagram or a photo grid — match the precedent already in the file for similar existing content).
- **UI chrome** (a toolbar button, a modal, a global diagram/feature): touches the page **once**, outside the three duplicated content blocks. Do not triplicate it.

## Step 2 — make the edit with a scripted, asserted string replacement

For anything beyond a one-line tweak, do **not** hand-edit the giant template-literal blocks with the `Edit` tool. Instead:

1. `Grep` for a short, unique anchor string near the target location, in each copy that needs the change.
2. Write a small Python script (to the scratchpad directory) that:
   - Reads the HTML file as UTF-8 text.
   - For each copy: `count = content.count(anchor); assert count == <expected>` (2 for ZH-static+ZH_MAIN_HTML, 1 for EN_MAIN_HTML, 1 for MD_CONTENT) — this catches the anchor being missing, or matching more places than intended, *before* it silently corrupts the file.
   - `content = content.replace(anchor, replacement)` then `assert content.count(replacement) == <expected>`.
   - Writes the file back.
3. Run the script via the `PowerShell` tool (not `Bash`) — `python <script path>` — since Bash-heredoc quoting mangles backslashes and non-ASCII text unpredictably on this Windows box.
4. For a **new image**: download via a small Python script with a proper `User-Agent` header (Wikimedia rate-limits anonymous urllib requests — retry with a delay and `?width=400` thumbnails, not full-res), verify the subject against an authoritative source, resize/recompress with Pillow, base64-encode, and splice in the same way as text.

## Step 3 — verify before considering it done

Run, in order:

1. **JS syntax check** — extract each `<script>…</script>` body with a regex and run `new Function(body)` in Node; must print `OK` for both blocks (the small inline theme-flash script, and the giant main script).
2. **Tag balance** — `grep -c '<details class="card">'` must equal `grep -c '</details>'` (243, unless the topic count itself changed).
3. **Playwright smoke test** — reuse/extend the scratch `pw/verify.js` pattern: launch Chromium, open the file via `file://`, exercise search / theme toggle / language toggle / reload-persistence, assert `consoleErrors.length === 0`. For a new interactive feature, add targeted assertions (element counts, click-then-check-resulting-state) rather than just eyeballing it.
4. **Screenshot check** — at minimum one mobile width (~375–390px) and one desktop width, in both themes if the change touches layout/CSS. Actually look at the image; don't just trust that "no error was thrown" means it looks right.

## Step 4 — publish

1. Copy the edited file over `index.html` (`cp "全方位緊急應變小卡 (2).html" index.html`) — GitHub Pages serves this one.
2. If topic content changed, regenerate the standalone `.md`: extract `MD_CONTENT` from the HTML with the same regex-and-unescape approach used elsewhere in this project, write it to `全方位緊急應變小卡 (1).md`. Skip this step for UI-chrome-only changes (confirm the extracted `MD_CONTENT` length is unchanged as a sanity check).
3. `git add` only the files that actually changed, commit with a message describing *why*, and push. Never `git add -A` blindly in this repo — it contains large generated PNGs and a duplicate HTML file that should only be re-added when they've actually changed.
