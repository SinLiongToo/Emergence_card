# 全方位緊急應變小卡 — All-in-One Emergency Reference Card

A single self-contained, **fully offline** HTML reference card covering **83 emergency, medical, wilderness, and disaster-preparedness topics** — from CPR and MARCH bleeding control to tarp-shelter pitching, Taiwan snake identification, and wartime go-bags. Traditional Chinese with an English toggle, dark/light theme, live search, and no internet connection required after the first load.

**Live version:** https://sinliongtoo.github.io/Emergence_card/

## Why offline-first

This is meant to be useful in the field — during a hike, a power outage, or an actual emergency — not just at a desk with Wi-Fi. Every image (knots, snake ID photos, shelter diagrams) is embedded directly in the page as base64 data, so the entire tool works with zero network access once it's loaded or saved to a device.

## Features

- **83 topics** across 8 categories: quick assessment, life-saving skills, trauma & wound care, medical & environmental emergencies, disaster & security response, wilderness & outdoor skills, and everyday preparedness
- **ZH / EN toggle** — every topic is fully translated, not machine-summarized
- **Dark / light theme**, remembered across visits
- **Live search** across all topic content, with match highlighting
- **Topic Connection Graph** — a visual map of how the 83 topics cluster into 8 main categories; tap any node to jump straight to that topic (falls back to a tap-friendly accordion list on phone-width screens)
- **Download as Markdown** — export the full reference as a single `.md` file
- Reference photos and diagrams sourced from Wikimedia Commons, U.S. Army field manuals (public domain), and verified species/technique identification — not AI-generated illustrations

## Files in this repo

| File | What it is |
|---|---|
| `全方位緊急應變小卡 (2).html` | The tool. Open it directly in any browser, or use the [live Pages version](https://sinliongtoo.github.io/Emergence_card/). |
| `index.html` | Identical copy, served by GitHub Pages. |
| `全方位緊急應變小卡 (1).md` | The same 83 topics as a plain Markdown document. |
| `全方位緊急應變小卡_總覽地圖.png` | A one-page poster mapping all 83 topics by category (stale — last regenerated at 81 topics). |
| `台灣蛇類辨識卡.png` | An extended photo reference card for Taiwan's venomous and commonly-confused non-venomous snakes. |

## Using it

Just open `全方位緊急應變小卡 (2).html` (or `index.html`) in a browser — no build step, no dependencies, no server required. Save it to your phone/laptop before you head out somewhere with no signal.

## Disclaimer

This card is a quick-reference aid. It does not replace proper first-aid training, professional medical care, or your local emergency services' guidance — when in doubt, follow local SOPs and seek professional help.
