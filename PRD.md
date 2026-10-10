---
slug: palette-of-the-day
date: 2026-10-11
author: taro
type: whimsy-ambient
register: B
status: building
---

# Palette-of-the-Day — PRD

## Source

Taro Weekly Whimsy Brief #009 (2026-10-10), candidate #2.
Taro's second pick if "build two" — distinct register from #1 Desk-Sand (Register B vs A). Synesthesia-text-to-brain appeal-library atom — first spin (un-spun for 31 days).

## Problem statement

Kim's appeal-library has the "synesthesia-text-to-brain" atom un-spun — the feeling of cross-sensory translation (one sense given the form of another). No prior Taro brief has attempted this atom.

Palette-of-the-Day is the atom re-cast for ambient everyday life: a passive browser tab that listens to the room (Web Audio API analyser + frequency-band decomposition) and renders the live sound as a continuously-drifting color palette across a warm-cream canvas, with one palette auto-saved per day at end-of-day. A 7-day mood-board strip below the canvas shows the saved palettes as small squares. No input from Kim (the mic listens; the palette drifts; the day is saved). Kim can re-roll today's palette (Register B tinkering affordance) and export today's palette as a CSS custom-property set she drops into a Figma file.

**Who:** Kim, solo, at her own desk. Secondary audience: design-curious IG followers who'd screenshot-share a daily-palette story.
**Need:** a passive ambient art-piece she can both use as a design seed and share as IG content.
**Today they:** have no way to turn "what the room sounded like today" into something visual and shareable.
**If we ship:** a browser tab that becomes a daily palette based on the room's sound, savable + exportable + IG-story-ready.
**Success signal:** Kim keeps the tab open through a working session, comes back to find a palette she wants to drop into a Figma file, clicks export, pastes the CSS block, and it works.
**Scope IN:** single-file static HTML + Web Audio API + frequency-band decomposition + CSS-rendered canvas + 7-day localStorage history + re-roll button + CSS-export-to-clipboard.
**Scope OUT:** auth, server, social feed, multi-week history, scoring, gamification.
**Constraints:** 8GB Mac, static-only, $0.01 cap (if no WASM needed; $0 default), no LLM calls.

## Why now

The atom has been waiting 31 days for a first spin. Secondary signal: Kim's standing IG-share pattern (sticky-note-constellation shipped 09-12 was the most recent shareable artefact) — the 7-day mood-board strip is the natural IG-story seed.

## Stack

- **Single-file static HTML** — `index.html` only, all CSS + JS inline
- **Web Audio API** — `AudioContext.AnalyserNode.getByteFrequencyData()` for 5-band decomposition
- **HTML5 Canvas** — for the drifting color circles
- **localStorage** — for the 7-day palette history
- **Clipboard API** — for CSS export
- **No frameworks** — vanilla JS, vanilla CSS, no build step
- **Hosting** — GitHub Pages, $0

## Register & ambient-only compliance (pitfall #41)

- **Register B** (playable, repeatable, tinkerable, shareable) — assigned BEFORE mechanic was generated
- **Ambient-only** (2026-09-11 hard rule) — the canvas reacts to room sound + time, two things already happening. No log moment. The re-roll button is a tinkering affordance (Register B grammar), not a log moment.

## Build plan

1. Write `index.html` with: warm-cream canvas (top 60% viewport), 5-band analyser decomposition, 5 overlapping drifting circles at ≥30fps, 7-day mood-board strip (bottom 40%), re-roll button, export-to-CSS button, mic-permission graceful degradation (time-only fallback).
2. Add `?day=30s` URL param to compress end-of-day for testing.
3. Smoke-test: page loads, AudioContext initialises, 5 bands decompose correctly, canvas drifts at ≥30fps, end-of-day saves to localStorage, re-roll regenerates from last 60s, export copies valid CSS block, mic-denied fallback renders deterministic time-of-day palette, no console errors.
4. Deploy to GitHub Pages via `agentSumi/palette-of-the-day` repo.
5. Verify MD5 byte-identical.

## Evals on success criteria (negative_check discipline)

| # | Criterion | Pass condition | Threshold | How verified |
|---|---|---|---|---|
| 1 | Page loads | HTTP 200 from `/` | 100% | `curl -sI` |
| 2 | Canvas renders | `<canvas id="palette">` exists with correct dimensions | 100% | DOM query |
| 3 | AudioContext initialises on first user gesture | `audioCtx.state === 'running'` after click | 100% | browser_exec |
| 4 | 5 frequency bands decompose | analyser.getByteFrequencyData() returns 5 distinct band averages | 100% | browser_exec |
| 5 | Canvas drifts at ≥30fps | rAF callback fires at ≥30Hz | 100% | browser_exec FPS counter |
| 6 | 7-day strip renders | 7 `<div class="day">` children, today's has `.today` class | 100% | DOM query |
| 7 | End-of-day saves to localStorage | `?day=30s` triggers save + reload shows it | 100% | browser_exec |
| 8 | Re-roll button regenerates | click re-rolls today's palette from last 60s | 100% | browser_exec |
| 9 | Export button copies valid CSS | clipboard contains `--palette-N: hsl(...)` lines | 100% | browser_exec clipboard |
| 10 | Mic-denied fallback works | simulate deny, palette still renders (time-only) | 100% | browser_exec |
| 11 | No console errors | `window.onerror` count === 0 | 100% | browser_exec |
| 12 | No LLM calls | no `openai`, `anthropic`, `completion`, `claude` strings in source | 100% | grep |
| 13 | MD5 byte-identical post-deploy | served MD5 == local MD5 | 100% | curl + md5sum |
| 14 | No dashboard / no scoring | no `score`, `progress`, `streak`, `level` strings in source | 100% | grep |

**SHIP verdict requires:** All 14 negative_checks pass.
**ITERATE:** re-derive the frequency-band-to-HSL mapping, re-run evals.
**SCRAP:** any criterion fails after 2 ITERATE loops.

## Demand-gate (pitfall #43)

"Kim (designer who thinks in palettes and screenshots-to-IG weekly — secondary audience: design-curious IG followers who'd share a daily-palette story) wants a passive ambient art-piece she can both use as a design seed (drop palette into Figma) and share as IG content — the synesthesia atom is un-spun in the appeal-library, this is the first attempt."

Named user ✓, named why-now ✓, no vacancy-claim-in-disguise ✓.

## Build type

`personal / whimsy-ambient / Register B` — single-file static HTML.
