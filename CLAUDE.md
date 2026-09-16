# All-in-One Emergency Reference Card

A single self-contained, offline-first HTML reference tool covering 83 emergency/wilderness/disaster topics, in Traditional Chinese with an English toggle. No build system, no external dependencies — one `.html` file you can open directly in a browser or serve as-is.

## Files

| File | Role |
|---|---|
| `全方位緊急應變小卡 (2).html` | **The source of truth.** Edit this file. |
| `index.html` | Exact copy of the above, for GitHub Pages (which needs a clean root filename). **Must be re-copied after every edit, before committing.** |
| `全方位緊急應變小卡 (1).md` | Plain-Markdown export of the topic content, regenerated from the HTML (see below) — not hand-edited. |
| `全方位緊急應變小卡_總覽地圖.png` | Static poster: all 83 topics grouped into 8 categories (**stale** — last regenerated at 81 topics). |
| `台灣蛇類辨識卡.png` | Static poster: Taiwan snake ID reference (topic 63 extended). |

GitHub Pages is live at `https://sinliongtoo.github.io/Emergence_card/` (source: `main` branch, `/` root).

## Critical structural fact: content exists in 3 (or 4) copies

Every topic card's content is duplicated:
1. The initial `<main id="mainContent">…</main>` block (rendered on load, Traditional Chinese).
2. The `ZH_MAIN_HTML` JS template-literal string (restores Chinese when toggling language back from English).
3. The `EN_MAIN_HTML` JS template-literal string (the English translation).
4. The `MD_CONTENT` JS template-literal string (plain-Markdown version of the *same topic content*, used by the "Download Markdown" button and to regenerate the standalone `.md` file) — topic-content edits need this too; **UI chrome** (header text, buttons, modals, diagrams) does **not** belong here.

**Any edit to a topic's content must be applied to all copies that contain it, or the tool will show stale/inconsistent content depending on language or export.** The reliable way to do this: write a small Python script that does a literal (non-regex) `str.replace` of an exact anchor string, `assert content.count(anchor) == <expected count>` before replacing, and `assert content.count(replacement) == <expected count>` after — this catches silent duplication/omission bugs immediately. Do **not** hand-edit the giant `EN_MAIN_HTML`/`ZH_MAIN_HTML` blocks with a text-editor `Edit` tool call for anything non-trivial; the anchors are long and duplicated, and it's easy to only patch one of the 2-3 copies. UI-chrome-only features (a new toolbar button, a modal, an SVG diagram) should instead live **once**, outside the three duplicated blocks (e.g. as a sibling of `<main>`).

## Images

All images are embedded as base64 `data:` URIs directly in the HTML — the tool must work with zero network access. When adding a new photo:
1. Source from Wikimedia Commons (`Special:FilePath/<name>?width=400` for a thumbnail) or another public-domain/CC source — never hotlink.
2. **Verify the species/subject identification against an authoritative source before embedding** — this tool includes safety-relevant content (snake ID, fire-starting), and past mistakes were caught this way (e.g. two snake species initially mis-mapped to the wrong Latin binomial).
3. Resize (~400–500px wide) and re-compress (JPEG quality ~70-78) with Pillow before embedding, to keep file size sane.
4. Follow the existing `.knotcard` / `.knotgrid` markup pattern for photo grids.

## The 8-category grouping

Reused across the overview poster and the in-app "Topic Connection Graph": 快速評估與分類, 特殊族群與情境, 基礎救命術, 外傷與傷口照護, 內科與環境急症, 災害與治安應變, 野外與戶外技能, 日常準備與防護. The exact topic-number-to-category mapping lives in `GRAPH_CATEGORIES` inside the HTML's `<script>` block — treat it as the canonical source if it needs to be reused again (it's been verified gap-free across all 83 topics).

## Verification workflow

There is no test suite — verification is a headless-browser pass done with Playwright (installed ad hoc into a scratch npm project, not a project dependency). After any edit:
1. `node -e "new Function(extractedScriptBody)"` on each `<script>` block — catches JS syntax errors cheaply, before ever opening a browser.
2. Grep-count `<details class="card">` vs `</details>` — must match (249 = 83 topics × 3 language/copy blocks; the mobile Topic Connection Graph's accordion-list renderer also contributes one matching literal-string pair in the JS source, so the live count in the file is 250 = 249 + 1).
3. A Playwright script that opens the file, exercises search / theme toggle / language toggle / localStorage persistence-after-reload, and asserts zero `console.error`/`pageerror` events.
4. Screenshot at a mobile width (~375px) and desktop width, in both themes — this UI is mobile-first and dark-mode-first by default.

See the project's Skill for the full step-by-step (`.claude/skills/emergency-card-editor.md`).
