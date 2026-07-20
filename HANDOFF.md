# connordore.com — session handoff

Paste this whole file into a new Cowork session before touching the site. Last updated 2026-07-20.

---

## What this is

Connor's portfolio site. **One self-contained file: `index.html` (884 lines).** No build step, no framework, no npm. Static HTML + CSS + vanilla JS. Edit the file, push, Vercel redeploys.

- Folder: `/Users/connordore/Desktop/Connorsite`
- Repo: `github.com/connorprecisionworks-sys/connordore` (branch `main`, push straight to main, no PRs)
- Target domain: **connordore.com** (Vercel). Old `Dorelabs.com` references are stale.
- Deploy: GitHub push triggers Vercel. Framework preset "Other", no build command, no output dir.

## Current state (as of 2026-07-20)

The site is TWO things and nothing else. Connor deliberately stripped it down.

1. **The dithered mountain scene** — full-viewport, fixed, scrolls away
2. **A minimal contact page** — his dithered photo, name, one line, four links

Everything else was built then deliberately scrapped: PRESS START gate, multi-section dashboard, Win95 desktop story, agent-permissions terminal, infection finale, signature, CD cold-open. **Do not resurrect any of it without asking.**

## Visual direction

Inspired by basement.studio's "Chronicles" (`https://chronicles.basement.studio/`) — 1-bit ordered dithering, CRT feel, retro-game type. Recognizable to people who know basement, novel to everyone else. Connor supplied their Apache-2.0 `shader-lab` repo (at `~/Downloads/shader-lab-main`) but the site does NOT use it at runtime; the dither is hand-rolled 2D canvas.

**HARD RULE: fully monochrome.** Black, white, grey only. The amber/orange accent was removed on purpose. `--accent` is now `#d6d6d6` (a grey) despite the stale comment on line 14 calling it "the orange". Any hue is a bug.

Fonts: Basement Grotesque + Geist + Bunker as local woff2 in `fonts/`. Press Start 2P and VT323 from Google.

## How the scene renders

Inside `#stage` (position:fixed). Source `<img>` and `<video>` elements are hidden in the DOM, drawn into `#dither` canvas each frame, then a Bayer 8x8 ordered dither pass runs per-pixel and blends at `DITHER_STRENGTH`.

Layer order, back to front:

1. **Clouds** — `assets/clouds.png`, mirror-tiled for seamless horizontal drift, mouse parallax
2. **Mountain** — `assets/mountain.png`, with `assets/lava.mp4` masked into the silhouette via `destination-in`
3. **Cross** — `assets/cross.mp4` masked by `assets/cross.png`, drawn BEHIND the foreground
4. **Foreground ridge** — `assets/foreground.png`, real alpha
5. Ambient asteroid, embers (grey), stars
6. **Name video** — `connor-name-v6.webm`, `mix-blend-mode:screen`

Each layer gets its own parallax amplitude for depth.

## Tunable knobs (line numbers as of this writing)

| What | Line | Value | Notes |
|---|---|---|---|
| Scene dither strength | 520 | `DITHER_STRENGTH=0.76` | 0 clean, 1 full 1-bit |
| Dot size | 583 | `SCALE = <700?1.5:1.25` | lower = finer/denser dots |
| Photo saturation | 730 | `SAT=0.30` | 0 grey, 1 full color |
| Photo contrast | 731 | `CONTRAST=0.86` | <1 flattens |
| Photo fade | 732 | `FADE=0.10` | veil toward mid grey |
| Photo resolution | 733 | `PW=760` | |

Connor iterated hard on these. He wanted finer/denser dots and a much LESS dithered photo. The photo is deliberately NOT 1-bit anymore, it is muted color, not black and white.

## The name video

`connor-name-v6.webm` — shader-lab export, "connor dore" in dithered blackletter. 1920x1205, VP9, **no alpha**, 1.98s.

Behavior: hidden on load, fades in at **3 seconds**, plays once, holds its final frame. Composited `mix-blend-mode:screen` (drops the black bg), `filter:grayscale(1)`, opacity .92.

**Important:** Connor uploaded a newer export (`shader-lab-2026-07-20T04-18-56.webm`) that is UNUSABLE — the name occupies only 550x180 inside a 1920x1205 frame, peak luminance 82/255, and it is red. Letterforms do not resolve even normalized. v6 is 1460x444 at luminance 202. If he sends a new one, verify size and brightness with ffprobe/PIL before wiring it.

Five other `connor-name-*.webm` files are tracked in the repo but unused. Harmless bloat.

## Contact page

Deliberate angle, per Connor verbatim: *"make fun of the current 'booking' page and make fun of the professionalism, the world has changed and I'm not conforming — don't say any of that but that's the angle."*

So: no headline, no pitch, no description of what he does, no booking widget. Just his dithered photo, `connor dore`, the line `just reach out.`, and four links (github, linkedin, phone, email). He explicitly killed a "no forms, no quick 15" line for being too on-the-nose. **Keep it understated. Do not explain the joke.**

Links hover: text scrambles through glyphs and resolves, plus a white tick on the left edge. Placeholder animation, he hasn't decided what he wants.

## Gotchas learned the hard way

- **`file://` taints the canvas.** `getImageData` throws, so the whole dither pipeline dies silently and you get a black screen. Never judge the site by double-clicking `index.html`. Serve it: `cd ~/Desktop/Connorsite && python3 -m http.server 8000`
- **Off-screen videos get throttled** and never decode. Video sources must stay in the viewport (behind the canvas, `z-index:-1;opacity:0.01`), NOT `left:-9999px`.
- **`position:fixed` with no `top`/`left`** inherits static-flow position. Caused an invisible-cursor bug.
- **File extensions must match exactly.** `.jpeg` != `.jpg`. Cost a debugging round.
- **The Cowork bash sandbox can CREATE files in mounted folders but CANNOT overwrite or delete existing ones** (permission denied). Use the Edit/Write tools for existing files.
- **The sandbox proxy blocks Higgsfield's CDN** (403 on CONNECT). Generated images cannot be auto-downloaded; Connor must save them manually.
- **Image generators cannot produce true alpha** — they bake in a transparency checkerboard. Use `remove_background`.
- The `#stage.entered` CSS rules (line ~155) are dead code from the removed gate. Nothing adds that class.

## Connor's working rules (from his brain file)

- **NO em dashes.** No AI-tell phrasing. No emojis unless asked.
- Direct, no fluff, no padding. Don't over-explain, don't ask permission for the obvious.
- Get it right in one pass. He hates revisiting the same thing twice.
- **Always name WHO runs a command** (Connor's terminal vs Claude Code vs Supabase SQL editor) **and WHERE** (which folder).
- Terminal commands must be paste-safe: no inline `#` comments in multi-line blocks, no smart quotes, one command per block.
- **End every technical response with a plain-English recap.** Label it "In plain English:".
- Verification is Connor's job. Report the diff, commit, push, build status. Do not simulate user flows.

## Immediate next task

**Add his projects to the site.** Not yet scoped. Ask before building: which projects, how much detail each, and whether a projects section undercuts the deliberately-minimal contact-page angle.

His projects (from his brain file, ranked): **Roastlytics** (coffee roastery OS, top revenue, launches Golden Bean Oct 2026), **Antfarm** (agent factory, top build priority), **Lexx AI** (legal AI, construction litigation), **Pergamum**, **Sola** (apologetics platform), **Onion** (secret), Prompt Library for The Arena, Intro to AI course, Canvas workload viewer.

Note: he does NOT want to be called an "AI consultant" (his call, buzzword). Brand is just "Connor Dore", no firm name.

## Files in the folder

- `index.html` — THE site. Everything.
- `assets/` — clouds, mountain, foreground, foreground-far, foreground-simple, cross.png/mp4, lava.mp4, asteroid, connor.jpeg, plus unused ant/scales/slate
- `fonts/` — BasementGrotesqueVF, Geist-Sans-VF, Bunker-Regular (woff2)
- `connor-name-v6.webm` — the live name clip
- `wordmark-lab.html` — local font-comparison tool, gitignored, not deployed
- `BUILD-SPEC.md`, `MONO-KICKOFF.md`, `ANIMATION-BRIEF.md`, `CODE-KICKOFF.md` — earlier specs, **largely superseded**, read for history only
- `preview-*.html`, `chronicles-intro.html` — dead prototypes

## Deploy

Connor runs these in his terminal, in the `Connorsite` folder:

```
cd ~/Desktop/Connorsite
```

```
git add -A
```

```
git commit -m "your message"
```

```
git push origin main
```

Vercel auto-deploys from `main`. Always tell Connor to verify on the `.vercel.app` URL before DNS, because the canvas and video only work over http(s).
