# connordore.com — Monochrome build kickoff

Palette + type override on top of `BUILD-SPEC.md`. Everything else in that spec still holds (page map, copy voice, project content, Cal.com booking, SEO prerender, accept checks, no-PR push-to-main flow). This file changes the LOOK only, per Connor 2026-07-15.

> DIRECTION UPDATE (2026-07-15, later same day): Connor chose the retro-dithered "PRESS START" world over the clean monochrome. See the `## RETRO-DITHERED DIRECTION` section at the bottom, which supersedes the clean-mono look. The clean-mono preview (`preview-mono.html`) is kept as the fallback / light-mode reference. The approved visual target is now `preview-retro.html`.

## What changed from the spec

- **Direction:** drop the sage editorial palette. New reference set = HF0, Palantir, Anthropic, Vercel, Linear, Resend. The through-line: monochrome, high-craft, technical precision, sharp grotesk type, hairline borders, generous whitespace, restrained micro-motion.
- **Default theme:** dark-native (like HF0 / Palantir / Linear), with a real light mode toggle (like Vercel / Resend). Both ship at launch. This supersedes the spec's "cream is canonical, dark is v1.1" line.
- **Look reference to match:** `preview-mono.html` in this folder is the approved visual direction. Build the real site to that feel.

## Design tokens (replace the sage tokens in tailwind.config)

Dark (default):
- bg `#0A0A0A`, bg-elev `#111111`
- text `#F5F5F5`, muted `#8A8A8A`, faint `#5A5A5A`
- hairline `rgba(255,255,255,0.10)`, hairline-strong `rgba(255,255,255,0.20)`
- accent `#FFFFFF`, chip `rgba(255,255,255,0.05)`

Light:
- bg `#FFFFFF`, bg-elev `#FAFAFA`
- text `#0A0A0A`, muted `#6B6B6B`, faint `#A0A0A0`
- hairline `rgba(0,0,0,0.10)`, hairline-strong `rgba(0,0,0,0.20)`
- accent `#0A0A0A`, chip `rgba(0,0,0,0.04)`

Rules: no color anywhere. Pure greyscale only. Anything that glows is a bug. Any hue outside black/white/grey is a bug.

## Type

- Display / headers: **Space Grotesk** (600/700), tight tracking (-0.02 to -0.045em).
- Body: **Inter** (400/500/600).
- Micro-labels, nav, tags, stat numbers, code-feel accents: **JetBrains Mono**, uppercase, letter-spacing 0.16-0.18em.

## The two 3D moments (recolored)

Keep both moments from `ANIMATION-BRIEF.md` but render them in pure greyscale. Simpler option that already matches the reference set and is lighter to ship: replace the hero "Assembly" clay with the **particle-field moment in `preview-mono.html`** (1800 monochrome particles that settle into the "CD" monogram, drift idle, and repel from the cursor). It reads Vercel/Linear, runs at 60fps, and degrades to a static poster under prefers-reduced-motion. Decide hero-clay vs particle-field before Phase 3; the Shelf project-reveal stays as specced but greyscale.

## Contact email

Preview uses `connordore36@gmail.com` (from the spec). Confirm which address is public before launch: `connordore36@gmail.com` or `connor.precisionworks@gmail.com`.

---

## PHASE 1 — paste this into Claude Code (running in the `Connorsite` folder)

Read `BUILD-SPEC.md`, `ANIMATION-BRIEF.md`, and `MONO-KICKOFF.md` in this folder in full before writing anything. `MONO-KICKOFF.md` overrides the palette, fonts, and default theme; everything else in `BUILD-SPEC.md` holds. Match the visual feel of `preview-mono.html` in this folder.

Phase 1 scope is scaffold plus homepage DOM shell with NO 3D yet:

1. Scaffold a Vite plus React 18 plus TypeScript app in this folder. Install react-router-dom, framer-motion, lenis, zustand, maath, detect-gpu, three, @react-three/fiber, @react-three/drei, @calcom/embed-react, tailwindcss. Do not install any postprocessing package.
2. Tailwind config with the MONO-KICKOFF tokens as named colors for both dark and light themes, driven by a data-theme attribute on html. Wire fonts Space Grotesk (display), Inter (body), JetBrains Mono (mono labels). Default theme is dark. Add a persisted light/dark toggle in the nav.
3. Set up react-router with 4 routes: / , /projects , /now , /about. Sticky nav (Work, Projects, Now, About plus a Book a call CTA button and the theme toggle), hairline bottom border appearing on scroll. Footer with real links only (email, GitHub, X, LinkedIn as placeholders Connor fills).
4. Build the homepage as a static DOM page, all 7 sections from BUILD-SPEC in order, with the approved copy, monochrome editorial styling matching preview-mono.html, and Framer Motion reveal-on-scroll (fade plus small y-translate, prefers-reduced-motion honored). Leave two clearly labeled empty section wrappers where the hero and Shelf 3D moments mount in Phases 3 and 4. Set up the Lenis smooth-scroll provider and the zustand scroll-progress store now.
5. Enforce copy rules: no em dashes anywhere, no AI-tell words (delve, unlock, elevate, game-changer). Run `grep -rEn "delve|game.chang|unlock|elevate" src/` and a separate grep for the em-dash character, and confirm both return nothing before pushing.

Acceptance for Phase 1: `npm run build` exits 0; homepage renders all 7 sections with real copy on desktop and at 390px with no horizontal scroll; nav, footer, and theme toggle work; Lenis smooth scroll active; grep for em dashes and AI-tell returns nothing. Report the file diff summary, commit hash, and confirm the push to main and a green build. Do not run live functional tests or drop test files.

---

## RETRO-DITHERED DIRECTION (approved 2026-07-15) — supersedes the clean-mono look

Connor's chosen direction after seeing the basement.studio Chronicles intro. Visual target = `preview-retro.html` in this folder. The whole site lives in a 1-bit dithered CRT world and stays there after the intro (no dissolve to clean).

### The idea, stated plainly
Adopt the basement.studio visual LANGUAGE (ordered dithering, CRT scanlines, halftone, pixelation, PRESS START gate, pixel/mono type) using their open-source tool, but with an ORIGINAL scene and identity. Someone who knows basement recognizes the technique; a newcomer has never seen it. Do NOT clone their mountain scene, their exact orange, or their copy. The centerpiece is Connor's particle-formed name, not a landscape.

### Signature identity (differentiators from basement)
- Accent: amber `#FF6A2C` (not basement's pure orange). Used sparingly: PRESS START, cursor-hot state, section corner tags, badges, key words. Everything else is greyscale.
- Credit line echoes their "CREATED BY BASEMENT.STUDIO" but true to Connor: `BUILT WITH ANTFARM` (his meta-tool literally built the site — best proof point).
- Scene: 1-bit dithered particle field that forms `CONNOR / DORE`, then drifts ambient after start. Not a mountain.
- Framing: portfolio as an arcade — HUD nav (`01 WORK / 02 NOW / 03 ABOUT`), projects as selectable "levels" with status flags, contact as `INSERT COIN TO CONTINUE`, footer as end-credits.

### Tokens
- amber `#FF6A2C`, amber-dim `#c94e1c`, ink `#EDEDED`, muted `#9A9A9A`, faint `#5B5B5B`, bg `#0A0A0A`, panel `rgba(9,9,9,0.86)`, line `rgba(255,255,255,0.12)`.
- Fonts (Google, all in preview): `Press Start 2P` (arcade UI, sparingly), `VT323` (terminal body), `Space Grotesk` 700 (big display / particle target), `Caveat` 700 (amber script overlay).

### How to build it for real (production, in the Vite + R3F repo)
The preview fakes the dither in 2D canvas (low-res render + 8x8 Bayer ordered dither + CSS scanlines/vignette + custom crosshair). For production, use basement's real toolkit:

- Package: `@basementstudio/shader-lab` (+ `three`, `@react-three/fiber` v9). Apache-2.0 licensed. Keep the LICENSE and a credit to basement.studio in the repo and site footer/source. Effect layers to use: `dithering`, `CRT`, `halftone`, `pixelation`, `posterize`. Run them as postprocessing over the R3F hero scene (the particle name), per the package's `useShaderLabPostProcessingSource` / postprocessing API.
- HARD CONSTRAINT: shader-lab's runtime requires WebGPU (`WebGPURenderer`). Most visitors will have it, but not all. Ship a capability check: if `navigator.gpu` is present, run the real shader-lab passes; if not, fall back to the 2D-canvas Bayer-dither pipeline from `preview-retro.html` (already written, portable, no WebGPU). A portfolio must never show a blank screen.
- PRESS START gate: intro overlay with fake load bar -> PRESS START (click or Enter) -> set a `started` flag that switches particles from the name to ambient drift and reveals the HUD + sections. Persist nothing; it is fast and part of the identity (Connor chose "stay in the retro world throughout," NOT gate-once).
- Performance gates from BUILD-SPEC still apply: 60fps target, `prefers-reduced-motion` renders a static dithered poster and disables flicker, mobile drops particle count and raises pixel size, only one GL context alive.
- Accessibility: the dither is decorative. All copy stays real DOM text at full contrast on the panels (do not render body copy through the dither). Custom cursor must not break keyboard nav; keep focus states visible in amber.
- SEO unchanged: prerender the 4 routes to static HTML; the WebGPU/canvas scene lazy-loads and never blocks first paint.

### Phase plan delta
Replace Phase 3 (hero "Assembly") and the palette parts of Phases 1-2 with the above. Phase 1 now = scaffold + retro tokens/fonts + HUD + PRESS START gate shell + all sections as real DOM (dithered styling, no live shader yet), matching `preview-retro.html`. Phase 3 = swap the CSS/2D-canvas placeholder scene for the real shader-lab WebGPU pipeline with the 2D-canvas fallback wired in. Everything else in BUILD-SPEC (routes, copy voice, Cal.com, SEO, accept checks) holds.

### Still open (confirm before Phase 1)
- Public email: `connordore36@gmail.com` or `connor.precisionworks@gmail.com`.
- Real GitHub / X / LinkedIn URLs (footer links are placeholder `#`).
