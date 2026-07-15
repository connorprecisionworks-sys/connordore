# Claude Code kickoff - connordore.com rebuild

How to use this file: open a Claude Code session INSIDE the `Connorsite` folder (this folder). Paste the Phase 1 prompt below into Claude Code. When Code reports a phase green, come back to Cowork with its output and I'll hand you the next phase prompt. Build sequence and accept checks live in `BUILD-SPEC.md`; the two 3D moments live in `ANIMATION-BRIEF.md`.

Push flow: straight to `main`, one commit per phase, no PRs. `npm run build` must exit 0 before every push. There is no database in this project, so no `supabase db push` gate applies.

---

## PHASE 1 - paste this into Claude Code (running in the `Connorsite` folder)

Read `BUILD-SPEC.md` and `ANIMATION-BRIEF.md` in this folder in full before writing anything. They are the build contract. Do not deviate from the locked palette, copy voice, or the two-3D-moments-only rule.

Phase 1 scope is scaffold + homepage DOM shell with NO 3D yet:

1. Scaffold a Vite + React 18 + TypeScript app in this folder. Install: react-router-dom, framer-motion, lenis, zustand, maath, detect-gpu, three, @react-three/fiber, @react-three/drei, @calcom/embed-react, tailwindcss. Do not install any postprocessing package.
2. Tailwind config with the Sage editorial tokens as named colors: cream #F1EDE2, ink #14140F, sage #6E8260, deepsage #4F5E44, forest #222820, hairline #E0DACC. Wire fonts Poppins (headers), Lora (pull quotes only), Inter (body). Add the sitewide paper-grain overlay layer that sits above where the WebGL canvas will mount.
3. Set up react-router with 4 routes: / , /projects , /now , /about. Sticky nav (Work, Projects, Now, About + a "Book a call" CTA button). Footer with real links only (email connordore36@gmail.com, GitHub, X, LinkedIn as placeholders I will fill).
4. Build the homepage as a static DOM page, all 7 sections from BUILD-SPEC in order, with real approved copy from the spec, editorial styling, and Framer Motion reveal-on-scroll (fades + small y-translate, prefers-reduced-motion honored). Leave two clearly-labeled empty section wrappers where the hero 3D "Assembly" and the "Shelf" 3D moment will mount in Phases 3 and 4. Set up the Lenis smooth-scroll provider and the zustand scroll-progress store now so the 3D phases can subscribe.
5. Enforce copy rules: no em dashes anywhere, no AI-tell words (delve, unlock, elevate, game-changer). Run `grep -rEn "delve|game.chang|unlock|elevate" src/` for AI-tell words, and separately grep the source for the em-dash character (U+2014) and confirm both return nothing before pushing. Use hyphens or restructure sentences instead of em dashes.

Acceptance for Phase 1: `npm run build` exits 0; the homepage renders all 7 sections with real copy and editorial styling on desktop and at 390px with no horizontal scroll; nav and footer work; Lenis smooth scroll active; grep for em dashes and AI-tell returns nothing. Report the file diff summary, commit hash, and confirm the push to main and a green build. Do not run live functional tests or drop test files - Connor verifies on his machine.
