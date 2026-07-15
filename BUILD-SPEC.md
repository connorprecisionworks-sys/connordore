# connordore.com - Build Spec v2 (Vite + React + R3F)

Owner: Connor. Scoped by Cowork, animations designed by Fable, built by Claude Code. Date: 2026-07-08.
This supersedes `tools-built/connordore-site/SITE-SPEC.md` (v1, Next.js/editorial-only) on STACK and MOTION. It keeps v1's copy, palette, positioning, and resolved decisions. Read `ANIMATION-BRIEF.md` (Fable) alongside this file - it is the design contract for the two 3D moments.

---

## The bar

An experienced technical founder scrolls the homepage and thinks: "this is beautiful, and this kid ships serious work." We hit that with editorial restraint plus exactly TWO showstopper 3D scroll moments, not a maximalist neon site. Reference feel: karrisaarinen.com's taste with one jaw-drop WebGL moment.

## Locked decisions (Connor, 2026-07-08)

1. Aesthetic: **editorial restraint + hero moment.** Sage editorial system for 90% of the page; 2 immersive scroll-linked 3D moments (hero + mid-page project reveal). Both defined in `ANIMATION-BRIEF.md`.
2. Projects: **both** - teaser moments woven into the homepage scroll (the "Shelf" 3D moment) AND a full `/projects` archive page.
3. Booking: **Cal.com embed** (Connor connects his Google Calendar). Sits in the contact section + a nav CTA.
4. Stack: **Vite + React + React Three Fiber.** No Next.js. Because Vite has no SSR, we add static prerender for SEO (see SEO section) - a portfolio must be findable.

## Carried forward from v1 (still locked)

- Palette "Sage editorial": Cream `#F1EDE2` bg, Ink `#14140F` text, Sage `#6E8260` accent, Deep Sage `#4F5E44` buttons/links/small text, Forest `#222820` inverted sections, Hairline `#E0DACC` borders. NO purple, NO glassmorphism, NO glows, NO rainbow gradients. Subtle paper grain overlay sitewide (sits ABOVE the WebGL canvas so 3D and DOM share one skin).
- Type: Poppins headers (tight tracking), Lora for pull quotes ONLY, neutral sans body (Inter).
- Positioning line: **"I build AI tools with business pros and take them to market."** Never "student," never "aspiring," never "passionate."
- Copy voice: Connor's brain hard rules. No em dashes. No AI-tell (delve, unlock, elevate, game-changer). Short declarative sentences. Every claim specific and true. Never invent users/revenue/metrics; if a number isn't real, write the true thing ("launching at Golden Bean, October 2026").
- Age: 16, framed ONCE on /about as velocity ("shipping real products at 16"), nowhere else.
- Contact email: `connordore36@gmail.com`.
- Proof: do NOT name the roastery partner (keep generic). Faith-driven work ethic INCLUDED on /about.

---

## Stack + scaffold

- Vite + React 18 + TypeScript.
- `@react-three/fiber`, `@react-three/drei`, `three`, `@react-three/postprocessing` NOT used (Fable: zero postprocessing).
- `framer-motion` (DOM motion), `lenis` (smooth scroll + progress source), `maath` (damping/easing), `detect-gpu` (tiering), `zustand` (tiny scroll-progress store).
- Tailwind CSS with Sage tokens in config (or CSS variables). Fonts self-hosted or Google Fonts: Poppins, Lora, Inter.
- Cal.com embed: `@calcom/embed-react`.
- Routing: `react-router-dom` (4 routes below).
- SEO: `vite-plugin-html` for base meta + `vite-react-ssg` OR `react-snap` prerender so each route ships static HTML. Sitemap + Open Graph image.
- Build gate: `npm run build` exits 0. Deploy: Vercel or Netlify (static output).

## Page map (4 routes, matches v1)

```
/           Hero(3D) -> Intro -> Shelf(3D projects) -> Currently/Past -> Proof -> Writing -> Contact(Cal.com)
/projects   Full archive, grouped Shipped / Building / Experiments
/now        Dated, <200 words, rewritten for public
/about      Story, the 16-year-old velocity frame, faith-driven ethic, longer bio, mailto CTA
```

Nav: Work, Projects, Now, About + one CTA button ("Book a call" -> Cal.com). Sticky, cream with hairline border on scroll. Footer: email, GitHub, X, LinkedIn - real links only, no dead icons.

## Homepage sections (in order)

1. **Hero - 3D "Assembly" moment.** See `ANIMATION-BRIEF.md` Moment 01. DOM headline "Connor" (Poppins, tight) + positioning line + one CTA "See the work" (scrolls). The 2,800-block clay structure builds on scroll behind/beside the headline.
2. **Intro.** 2-3 short sentences. Who he builds for (SMB owners) and the through-line (systems that compound). One Lora pull quote allowed here.
3. **Shelf - 3D project reveal moment.** See `ANIMATION-BRIEF.md` Moment 02. Camera walks past 4 clay sculptures (Roastlytics, Antfarm, Lexx AI, The Arena); DOM text swaps per project. Each: problem one-liner, impact/traction line, 3-4 stack tokens, live link (or GitHub/demo). Copy from "Project content" below.
4. **Currently / In the past.** Karri Saarinen pattern, no dates. Currently - Antfarm, Roastlytics, The Arena. In the past - pull 2-3 from `tools-built/projects.md` at build time.
5. **Proof.** Specific only: Golden Bean roastery partnership (generic, do NOT name partner), Who Else / Genesis Studios @ ACU affiliation, GitHub link. NO logo walls, NO anonymous quotes. Cut the section if it would be thin.
6. **Writing.** 3-5 linked posts (X/LinkedIn) from `content/drafts/` published items. If fewer than 3 exist, ship hidden behind a flag and note it.
7. **Contact.** Its own section, not footer. Line: "Want to build something?" + `connordore36@gmail.com` visible mailto + **Cal.com inline embed** to book a call. Exactly one booking widget.

CTA rule from v1 relaxed for the booking flow: primary action sitewide is "Book a call" (Cal.com) + the visible mailto. Keep it to those two actions; no forms.

## Project content (Shelf + /projects)

- **Roastlytics** - "An operating system for coffee roasteries: green inventory, roast tracking, wholesale." / "Built with a roastery partner; launching at Golden Bean, October 2026." / Next.js, Supabase, Tailwind.
- **Antfarm** - "An agent factory: a desktop tool where an AI crew plans, builds, reviews, and ships code through a gated pipeline." / "The tool this site was built by." / Tauri, Rust, React, Claude. (This impact line is the site's best proof point - keep it.)
- **Lexx AI** - "Legal AI for construction litigation, built alongside practicing attorneys." / status line current at build time. / Next.js, Claude.
- **The Arena (Who Else)** - "An AI-judged pitch competition for high-school builders." / "Built the engine." / role framing.

`/projects` = everything, grouped Shipped / Building / Experiments, one line + link each. Velocity is the message.

## SEO (because Vite has no SSR)

- Prerender all 4 routes to static HTML (react-snap or vite-react-ssg).
- Per-route `<title>` + meta description + Open Graph tags. One OG image (sage editorial, "Connor - builds AI tools with business pros").
- `sitemap.xml`, `robots.txt`, canonical URLs.
- The 3D canvases lazy-load and never block first paint; text content is real DOM so crawlers read it.

## Definition of done (accept checks)

- `npm run build` exits 0; prerender produces static HTML for all 4 routes.
- Every page passes the 10-point `prompts/page-depth-audit.md` pass; `/qa` run on all 4 routes with evidence in the Builder report (skills-in-loops rule).
- Zero em dashes and zero AI-tell in rendered copy: `grep -rE "delve|game.chang|unlock|elevate" src/` returns nothing; em-dash grep over rendered source returns nothing.
- Palette conformance: no `#863bff`, no `backdrop-blur`, no rainbow gradient classes; sage tokens present. **In-canvas rule (Fable): any color outside the 5-step sage ramp is a bug; anything that glows is a bug.**
- 3D: both moments hit 60fps on Intel Iris Xe class; `prefers-reduced-motion` renders static posters; mobile fallbacks per `ANIMATION-BRIEF.md`; scroll-up perfectly reverses scroll-down; only ONE GL context alive at a time.
- Cal.com embed loads and books; mailto present; all external links resolve (no 404s).
- Mobile: no horizontal scroll at 390px.
- Theme: ship the cream editorial theme as canonical. Forest dark theme is v1.1, not required for launch (logged exception to the page-depth-audit theme point).

## Build sequence (for Code)

1. Scaffold Vite+React+TS in the Connorsite folder. Tailwind + sage tokens + fonts. Router with 4 routes. Lenis + zustand progress store. Register in `ant-farm-registry.json` if applicable. Commit.
2. Homepage DOM shell: all 7 sections with real copy, editorial styling, Framer Motion reveals. NO 3D yet. Passes depth audit as a static page. Commit.
3. Moment 01 "Assembly" (hero 3D) per `ANIMATION-BRIEF.md`, GPU-attribute interpolation from day one. Poster + reduced-motion + mobile fallback. Commit.
4. Moment 02 "The Shelf" (project reveal 3D) per `ANIMATION-BRIEF.md`. Reuse StudioRig. Fallbacks. Commit.
5. `/projects`, `/now`, `/about` pages. Cal.com embed in contact. SEO prerender + meta + sitemap + OG. Commit.
6. Depth pass: page-depth-audit on all pages, `/qa` evidence, accept checks green. Report to Connor.
7. Connor (his UI steps): GitHub repo + Vercel/Netlify project + connordore.com domain. Deploy.

Salvage from old `connordore-site/`: the three project descriptions (already adapted above) and nothing else; archive that folder after launch.
