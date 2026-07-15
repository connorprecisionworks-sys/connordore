# connordore.com - Signature 3D Moments Design Brief

By Fable, 2026-07-08. The design contract for the two scroll-linked WebGL moments. Everything else on the site is DOM + Framer Motion. Pair with `BUILD-SPEC.md`.

## Shared foundations (read first, applies to both moments)

- **One visual language: matte clay.** Every 3D surface is `MeshStandardMaterial`, roughness 0.85-0.95, metalness 0. Zero postprocessing passes. No bloom, no DOF, no chromatic anything. The expensive look comes from light and shadow on matte sage forms against cream.
- **Palette in 3D:** a 5-step monochrome-sage ramp and nothing else: `#8FA080` (sage lifted toward cream), `#6E8260` (Sage), `#5D6E51`, `#4F5E44` (Deep Sage), `#222820` (Forest, shadow-side accents only). Backdrop is the page itself: transparent canvas over Cream `#F1EDE2`. The CSS paper-grain overlay sits ABOVE the canvas so 3D and DOM share one skin.
- **Color fidelity:** `gl={{ antialias: true }}`, `toneMapping: THREE.NoToneMapping`, sRGB output. Tune light intensities to avoid clipping rather than letting ACES shift the sage hue (ACES pushes greens muddy-yellow).
- **Lighting rig (identical in both moments):** one directional key from upper-left (intensity ~1.6, slightly warm white `#FFF9EE`), one `hemisphereLight` (sky Cream, ground `#3A4234`, intensity ~0.5). Shadows via drei `<ContactShadows>` only (soft ellipse under object, opacity ~0.35, blur ~2.4). No shadow maps on the lights.
- **Camera:** FOV 35 everywhere. Compressed, product-photography perspective. Never wide-angle.
- **Scroll:** Lenis drives the page. A tiny store (zustand or a mutable ref) exposes each section's normalized progress 0->1 from `getBoundingClientRect` in the Lenis raf. Do NOT use drei `ScrollControls` (it hijacks the scroll container). Inside `useFrame`, chase target progress with `maath/easing.damp` (lambda ~4) so scrubbing feels weighted.
- **Loading:** each Canvas is `React.lazy` + mounted by IntersectionObserver (rootMargin ~100%). Until the GL context is warm, show the moment's poster image and crossfade to live canvas over 400ms. First paint is never blocked; no pop-in.
- **`prefers-reduced-motion`:** both moments render their poster (the final/hero state) as a static image. No pin, no scrub. Content and copy never depend on animation.

---

## Moment 01 - HERO: "Assembly"

**Concept.** Thousands of small matte-clay blocks hang in loose, suspended disorder. As the visitor scrolls, blocks travel to their positions and lock (bottom course first, then upward) into a clean stepped stack of strata: a quiet monolith of compounding layers. Scroll back up and it deterministically un-builds. The metaphor: parts, ordered by a system, compound into something that stands. It is also literally what Antfarm did to this site. Not an explosion, not a particle storm: construction, calm and inevitable. The reverse-scrub precision tells a technical founder this was engineered, not templated.

**What the viewer sees.**
- 0%: Cream page. Left half: "Connor" + one-line positioning (Poppins, tight, Ink). Right half: a drifting cloud of ~2,800 sage blocks, gently bobbing (curl-noise idle, millimeter amplitude). Soft contact-shadow ellipse hints where the structure lands.
- 50%: Lower two-thirds locked into crisp stepped slabs, each course a different sage step. Remaining blocks mid-flight on curved paths. Camera has orbited ~12deg and dollied in slightly. Shadow sharpened and grown.
- 100%: Finished stepped stack (a ziggurat of ledger slabs, ~4:5 aspect, ~7 courses, each inset from the one below) at a 3/4 editorial angle. One micro-settle (2% overshoot, damped). Section unpins; page scrolls into intro copy. Idle: near-still, one very slow breathing light angle (+/-2deg over 20s).

**3D content.** `THREE.BoxGeometry` with slight baked bevel (or `RoundedBoxGeometry` lowest segments), non-uniform brick proportions (~1 x 0.45 x 0.7 units), ONE `InstancedMesh`, ~2,800 instances (~60-90k tris, one draw call). Per-instance attributes baked at build with seeded PRNG (identical every visit): `aStart` (scattered curl-noise sphere position), `aTarget` (slot in stepped grid), `aDelay` (stagger keyed to height: low courses lock first), `aTilt` (random start rotation), `aColor` (index into 5-step ramp; lower courses skew Deep Sage/Forest, upper skew lifted sage). Camera: starts r=14 low/off-axis, ends r=10 at 3/4 hero angle. Dolly + orbit only, no roll.

**Scroll choreography.** Hero pinned for 180vh (CSS sticky inner viewport, no JS pinning). Global progress linear from scroll (scrub honesty). All easing lives per-instance: each block remaps global progress through its `aDelay` window, `smoothstep`-cubed ease on position + slerp on rotation. Net: an organic build wave climbing the structure, zero springs, perfectly reversible. Camera chases scroll-derived target with `maath` damping (the one place damping is allowed). Headline (DOM, Framer Motion): opacity/translate tied to same progress; positioning line swaps at ~70% to the one Lora pull quote.

**R3F implementation.** Libraries: `@react-three/fiber`, `@react-three/drei` (ContactShadows, PerformanceMonitor), `lenis`, `maath`, `three`, `detect-gpu`. No postprocessing package. KEY TECHNIQUE - GPU interpolation: do NOT set 2,800 instance matrices per frame on the CPU. Extend `MeshStandardMaterial` via `onBeforeCompile` (or `three-custom-shader-material`): vertex shader receives `aStart/aTarget/aDelay/aTilt` attributes + a single `uProgress` uniform and computes per-instance eased position + rotation on the GPU. Per-frame JS cost = one uniform write. Lighting/shadows/standard pipeline stay intact.
Component structure: `<HeroSection>` (DOM sticky pin, headline, poster) / `<HeroCanvas>` (lazy Canvas, DPR clamp, PerformanceMonitor) / `<AssemblyField>` (InstancedMesh + custom shader material, subscribes to progress ref in useFrame) / `<StudioRig>` (shared lights + ContactShadows, reused in Moment 02) / `useSectionProgress(ref)` (Lenis hook, returns mutable progress ref).

**Mobile + reduced-motion.** Mobile (GPU tier >=2): same scene, 700 instances, DPR clamp 1.5, ContactShadows -> static baked radial shadow plane, canvas full-bleed behind headline, pin shortened to 120vh. Mobile low-tier + reduced-motion: the poster (pre-rendered WebP of the 100% assembled state, exported once from the real scene via `canvas.toBlob`, ~60KB), with 12px Framer parallax drift on mobile, fully static under reduced-motion.

**Perf.** Budget: 1 instanced draw + ContactShadows (~2 draws), <=90k tris, DPR <=2, per-frame JS <0.5ms, 60fps on Intel Iris Xe. PerformanceMonitor degrades DPR 2 -> 1.5 -> 1.25 before touching instance count. BIGGEST RISK: CPU instance interpolation (`setMatrixAt` x 2,800 x 60fps) will melt mid laptops. Build it GPU-side from day one; the CPU version never gets fixed later.

---

## Moment 02 - MID-PAGE PROJECT REVEAL: "The Shelf"

**Concept.** Four shipped products displayed like objects in a design museum. A single long cream plinth holds four small abstract clay sculptures (one per flagship); scrolling walks the camera laterally down the shelf, dwelling at each piece while its case-study text swaps in the DOM beside it. Software as physical artifact: tactile, confident, zero spectacle. The statement is curatorial - this person has a body of work and treats it with the seriousness of objects.

The four objects (all matte sage clay, abstract, no logos):
- **Roastlytics** - ribbed horizontal cylinder (roasting drum): lathe-like fins, `#6E8260`, industrial and warm.
- **Antfarm** - modular lattice: ~400 instanced micro-cubes forming a half-built orthogonal colony, mixed ramp colors; deliberately echoes the hero's Assembly at desk scale (the site quietly says the hero and this object are the same idea).
- **Lexx AI** - a codex: 9 thin stacked slabs, fanned 3deg like a case file mid-review, Deep Sage with Forest edges.
- **The Arena** - stepped amphitheater ring: concentric rising annular steps with one open notch, `#5D6E51`.

**What the viewer sees.** 0%: section pins, camera at shelf's left end, Roastlytics centered right-of-frame; left DOM column "01 / Roastlytics" + title + one-liner + metric + link, framed by hairline rules. 50%: camera mid-travel between objects 2 and 3, Antfarm sliding off left, Lexx arriving; DOM text mid-crossfade; travel brisk, dwells slow. 100%: settled on The Arena, open notch to camera, "04 / The Arena" resolved; section unpins to footer/contact.

**3D content.** Each object <=12k tris (lattice instanced: 400 boxes ~5k tris, 1 draw). Plinth = one long flattened box, cream-tinted sage-grey. Total scene <=50k tris, <=7 draws. Same `<StudioRig>` as hero. The key light subtly re-aims toward the dwelled object (damped lerp of directional light position) - the curator's lamp, attention without glow. Camera: lateral x-track along the plinth at fixed height + gentle 6deg yaw toward the active object during dwells. FOV 35.

**Scroll choreography.** Pinned 400vh (sticky). Progress remapped through a piecewise dwell curve: four plateaus (~60% of scroll total) joined by three eased travels (~40%). Implement as a keyframed `smoothstep` segment map. During a dwell, residual scroll micro-delta drives a slow turntable of the active object (+/-10deg). DOM text (Framer `AnimatePresence`): index/title/copy crossfade + 12px y-shift at each segment midpoint, direction-aware.

**R3F implementation.** Same stack, nothing new. Reuse `<StudioRig>`, `useSectionProgress`, the DPR/PerformanceMonitor shell. Technique: scroll-linked camera dolly with keyframe remap (deliberately the simplest technique; craft here is 90% composition + easing curves). One `useFrame`: read progress ref -> remap through segment curve -> damp camera x/yaw -> damp key-light target -> set active object turntable. All four objects always mounted (tiny scene); frustum culling handles the rest.
Component structure: `<ShelfSection>` (DOM sticky, text column, shared segment-map constant) / `<ShelfCanvas>` -> `<StudioRig>` + `<Plinth>` + `<DrumObject>`/`<LatticeObject>`/`<CodexObject>`/`<ArenaObject>` (dumb static groups) / `<ShelfCameraRig>` (the one animated component) / `useActiveProject(progressRef)` (derives 0-3 index for DOM text).

**Mobile + reduced-motion.** Mobile: the lateral dolly needs width it lacks - don't fight it. Replace with a DOM-only horizontal snap-scroll of four cards, each carrying a pre-rendered object render (WebP posters, transparent bg, ~40KB each) with Framer tilt-on-drag (+/-3deg). Same text, same order - a designed fallback, not a degradation. Reduced-motion: four stacked static rows (poster left, text right, hairline rules), no pin.

**Perf.** Budget: <=50k tris, <=7 draws, one canvas, per-frame JS <0.3ms. Comfortably 60fps. BIGGEST RISK: pin jank, not GPU. A 400vh sticky section with scroll-linked camera is where cheap sites stutter. Non-negotiable mitigations: CSS `position: sticky` (never JS transform pinning), Lenis smoothing, all camera/light motion damped (no direct progress assignment), and the two canvases NEVER mounted simultaneously (IntersectionObserver unmounts the hero context before the Shelf warms - one GL context alive at a time).

---

## Taste rules (give to the engineer verbatim)

1. If a color outside the 5-step sage ramp appears in the canvas, it's a bug.
2. If anything glows, it's a bug.
3. If scroll-up doesn't perfectly reverse scroll-down, it's a bug.
4. The 3D never carries information - every fact lives in the DOM. The 3D carries conviction.
5. When in doubt, remove motion. The hero earns its 2,800 blocks by being the only place on the site where that many things ever move.

In plain English: the site gets exactly two 3D moments - the top of the page, where thousands of little clay blocks build themselves into a monument as you scroll, and a middle section where the camera walks you past Connor's four projects displayed like sculptures on a museum shelf. Everything is earthy sage-and-cream, driven by scrolling, fast on normal laptops, and quietly says: this person builds systems that stack up.
