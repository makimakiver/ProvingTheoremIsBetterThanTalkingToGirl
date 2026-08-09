# Handoff: Math is fun — ECDLP research road

## Overview
"Math is fun" is a personal academic workspace for research on the elliptic-curve discrete logarithm problem (ECDLP) and quantum order finding (Shor). The shipped concept (turn 5 in the canvas) is **The Road**: a black screen with a winding dotted path of checkpoints. Each uploaded PDF becomes a checkpoint represented by a custom sticker character. Clicking the character zooms it smoothly into an Avara-style detail view: character large on the left, spec panel on the right, with an in-page render of the actual PDF.

## About the Design Files
The files in this bundle are **design references created in HTML** — prototypes showing intended look and behavior, not production code to copy directly. The task is to **recreate these designs in your codebase's environment** (React/Next.js, Vue, etc.) using its established patterns and libraries. If no environment exists yet, pick the framework you'd use for the real product (a React SPA or Next.js app is the natural fit — the prototype's logic is already React-class-shaped).

`Math is fun - ECDLP Notebook.dc.html` is a design-exploration canvas containing five iterations (newest at top). **Turn 5 (id `5a`, "The Road") is the design to implement.** Turns 1–4 (paper library, dark app shell, minimal blog, tile index) are earlier explorations kept for reference — do not implement them unless asked.

## Fidelity
**High-fidelity.** Colors, type, spacing, animation timings and interaction behavior in section `5a` are final. Recreate pixel-perfectly.

## Screens / Views

### 1. The Road (home)
- **Purpose**: browse research checkpoints; one checkpoint per ingested PDF.
- **Canvas**: 1100 × 760 in the mock; implement full-viewport.
- **Background**: theme `bg` (dark `#000`, light `#F4F1EA`), `transition: background .4s`.
- **Road**: SVG path `M550 60 C 550 160, 300 220, 335 380 C 370 540, 550 600, 550 720`, stroke = theme `road`, width 2, `stroke-dasharray: 1 14`, `stroke-linecap: round` (reads as dotted).
- **Header (centered, top 34px)**: logo glyph (circle + curve arc SVG, stroke = theme `ink`, opacity .6) + `MATH IS FUN — THE ROAD` in JetBrains Mono 11px, letter-spacing .14em, color = theme `sub`; below it the tagline *"one character per theorem survived"* in Spectral italic 12.5px, color = theme `faint`.
- **Theme toggle (top-right 26/28px)**: pill, 1px border = theme `border`, radius 20px, padding 7×12, background = theme `chipBg`; icon ☾/☀ 12px + label DARK/LIGHT (JetBrains Mono 10.5px, letter-spacing .1em, color = theme `sub`). Click toggles theme.
- **Checkpoint 01** (absolute; sticker at 240,300 / 170×170, label block at 432,300):
  - Sticker: the character PNG (`pasted-1786300224040-0.png`, transparent), `object-fit: contain`, `filter: drop-shadow(0 0 28px rgba(90,200,232,.25))`, ambient bob animation: `@keyframes bob { 0%,100% { translateY(0) rotate(-1.5deg) } 50% { translateY(-10px) rotate(1.5deg) } }`, 5s ease-in-out infinite.
  - Hit area: only a centered circle 64% of the sticker box is clickable (`border-radius: 50%`); the transparent margins pass clicks through. Hover on the hit area scales the image to `scale(1.1)` with `transition: transform .3s ease` (no other motion).
  - Label: title "Shor's Algorithm in Qiskit" (Space Grotesk 500 16px, theme `ink`), meta `checkpoint 01 · PDF · 8 Aug` (JetBrains Mono 11px, theme `accent`), note in Spectral italic 13px, theme `faint`.
- **Next checkpoint** (absolute 470,620): dashed circle 96px (1px dashed theme `border`) with "next" (JetBrains Mono 10px, theme `faint`); label "next checkpoint" (Space Grotesk 500 14px, theme `sub`) + "upload a PDF to unlock" (JetBrains Mono 11px, theme `faint`).

### 2. Checkpoint detail (zoom view)
- **Trigger**: click the sticker. This is a shared-element transition, not a modal swap (see Interactions).
- **Layout**: full-screen overlay, background = theme `bg`, CSS grid `1fr 420px`.
- **Left region**: sticker centered at 400×400 (same element, moved/scaled); "← the road" top-left (28px inset, JetBrains Mono 13px, theme `sub`); bottom-center pager: pill border 1px theme `border`, radius 20px; ‹ disabled (color = theme `disabled`, background = theme `disabledBg`, `cursor: not-allowed`); › enabled (theme `ink`; hover background `rgba(90,200,232,.12)`, color `#5AC8E8`). Above the pager, a "coming soon…" line (JetBrains Mono 12px, theme `sub`) fades via opacity transition .35s.
- **Right panel**: background = theme `panel` (dark `#0A0F18`, light `#EFEBE1`), padding 52px 44px 36px, scrollable. Two swappable states:
  - **Specs (default)**: H2 "Shor's Algorithm in Qiskit" (Space Grotesk 500 32px, letter-spacing −.01em, theme `ink`); paragraph (Spectral 300 15.5px / 1.75, theme `body`); spec rows separated by 1px theme `hairline`, each `padding: 15px 0`, label left (Space Grotesk 13px, theme `faint`) / value right (13px, theme `ink`; mono for numeric rows):
    Format `PDF · 23 pages` · Thread `Order finding · Shor` · Instance `N = 15, a = 2` (mono) · Peaks `0 · 64 · 128 · 192` (mono) · Result `r = 4 → 15 = 3 × 5` (mono, theme `accent`) · Status `Checked on hardware`.
    Footer pinned to bottom: primary button "Read the PDF" (background `#5AC8E8`, text `#02121C`, Space Grotesk 600 13px, radius 6px, padding 12×20; hover `#7CD8F2`) + secondary link "open in new tab ↗" (JetBrains Mono 11.5px, theme `sub`).
  - **PDF view**: header row `THE PDF · 23 PAGES` (JetBrains Mono 11px, letter-spacing .12em, theme `faint`) + "← details" (theme `accent`); while rendering show `rendering PDF… n/23`; pages render as full-width images (white background, radius 4px, `box-shadow: 0 2px 10px rgba(0,0,0,.35)`), stacked with 14px gap.

## Interactions & Behavior
- **Shared-element zoom**: the sticker is ONE absolutely-positioned element living above both screens. Closed: left 240px, top 300px, 170×170. Open: left 140px, top 180px, 400×400. Transition `left/top/width/height .65s cubic-bezier(.4,0,.2,1)`. The overlay (background + panel) is always mounted, fading `opacity .45s ease .1s` with `pointer-events` gated. The bob animation continues throughout.
- **Open**: click sticker hit-circle → overlay fades in, sticker glides to detail position, PDF rendering starts (idempotent guard).
- **Close** (zoom out): click "← the road" OR click anywhere outside the sticker/panel (overlay backdrop has the close handler; panel and pager stop propagation).
- **Pager**: ‹ inert (first checkpoint). › shows "coming soon…" for 1.8s (timer resets on repeat clicks).
- **Hover**: sticker scales 1.1 (300ms); pager › tints cyan; buttons as specified.
- **PDF rendering**: client-side with pdf.js (`pdfjs-dist@4.10.38`), scale 1.3, each page drawn to a canvas → JPEG data-URL (quality .85), appended incrementally so pages stream in one by one; progress counter updates per page. Keep "open in new tab ↗" as escape hatch. Do NOT use `<iframe>`/`<object>` PDF embeds — browser viewers get blocked (Brave) and don't run in sandboxed contexts.
- **Theme toggle**: swaps the token set below; all colors transition naturally (background has .4s transition).

## State Management
- `theme: 'dark' | 'light'` (persist in localStorage in the real app).
- `checkpointOpen: boolean` — drives overlay opacity/pointer-events and the sticker's position/size values.
- `pdfView: boolean` — specs vs PDF pages in the right panel.
- `stickerHover: boolean` — hover scale.
- `comingSoon: boolean` + 1.8s timeout — pager toast.
- `pdfPages: {n, url}[]`, `pdfLoading`, `pdfDone` — incremental pdf.js output; render started once (guard flag).
- Data model for real app: checkpoints = ordered list of `{ id, title, summary, specs {format, thread, instance, peaks, result, status}, pdfUrl, stickerUrl, date }`.

## Design Tokens
Fonts: **Space Grotesk** (UI: 400/500/600), **Spectral** (body/serif: 300/400, italic), **JetBrains Mono** (meta/numbers: 300/400/500) — all on Google Fonts.

Dark theme: bg `#000` · ink `#fff` · body `rgba(255,255,255,.78)` · sub `rgba(255,255,255,.5)` · faint `rgba(255,255,255,.35)` · border `rgba(255,255,255,.18)` · hairline `rgba(255,255,255,.08)` · road `rgba(255,255,255,.14)` · accent `#5AC8E8` · panel `#0A0F18` · chipBg `rgba(255,255,255,.04)` · disabled `rgba(255,255,255,.18)` · disabledBg `rgba(255,255,255,.03)`.

Light theme: bg `#F4F1EA` · ink `#23241f` · body `rgba(35,36,31,.8)` · sub `rgba(35,36,31,.55)` · faint `rgba(35,36,31,.4)` · border `rgba(35,36,31,.25)` · hairline `rgba(35,36,31,.12)` · road `rgba(35,36,31,.2)` · accent `#2E7D96` · panel `#EFEBE1` · chipBg `rgba(35,36,31,.04)` · disabled `rgba(35,36,31,.2)` · disabledBg `rgba(35,36,31,.04)`.

Accent actions: `#5AC8E8` (hover `#7CD8F2`), on-accent text `#02121C`. Character glow: `drop-shadow(0 0 28px rgba(90,200,232,.25))`.
Radii: buttons/cards 6px, pills 20px, PDF pages 4px. Motion: zoom .65s `cubic-bezier(.4,0,.2,1)`; fades .35–.45s; hover .3s; bob 5s.

## Assets
- `assets/sticker-shor.png` — the checkpoint character (user-provided, transparent PNG, 1254×1254). Currently baked in; the real app should let users upload one per checkpoint.
- `assets/shor-algorithm-qiskit.pdf` — the actual paper behind checkpoint 01 (23 pages).
- `assets/pdf-text.json` — pre-extracted per-page text of that PDF (useful for search/summaries).
- Logo glyph: inline SVG (circle + elliptic-curve arc), stroke currentColor — recreate in code, no asset file.

## Files
- `Math is fun - ECDLP Notebook.dc.html` — the full design canvas. **Section id `5a` = The Road + detail view (implement this).** Template markup is inside `<x-dc>`; behavior is in the `class Component` script at the bottom (positions, timings, theme tokens, pdf.js rendering — lift values from there).
- `image-slot.js` — drag-and-drop image placeholder web component used by earlier explorations; reference only.
# ProvingTheoremIsBetterThanTalkingToGirl
