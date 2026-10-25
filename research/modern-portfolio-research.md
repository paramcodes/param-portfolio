# Modern Portfolio: Stack Research + The Emil Kowalski System

Research compiled October 2026. Two questions: **what stack** a unique, modern portfolio should use, and **what the Emil Kowalski skill suite (plus the other installed skills) tells us to build differently**. Sources are primary (official docs, the actual sites, the actual skill files).

---

## 1. TL;DR

- **Stack:** Astro 7 (static-first, zero-JS default, typed content collections) + a **CSS-native animation core** (view transitions, scroll-driven animations, `@starting-style` — all cross-browser in 2026) + **Motion** in small islands for springs/gestures + free **GSAP SplitText/ScrollTrigger** for one signature typographic moment + **Cloudflare Pages**. Alternatives: Next.js 16 (if you want the React ecosystem end-to-end) or SvelteKit 3.
- **The differentiator is not the stack.** Every good 2026 portfolio uses the same libraries. The differentiator is **taste**, and the Emil Kowalski system — installed locally as ~10 skills — is a machine for encoding taste into buildable rules. The unique portfolio comes from applying its restraint rules (frequency gate, asymmetric timing, clip-path, drag physics, "confirm in place") where generic portfolios do the opposite (animate everything, fade-slide-up every section, toast everything).
- **Biggest 2026 shift:** most "wow" animation is now plain CSS. GSAP is 100% free (incl. SplitText). Same-document view transitions are Baseline. Native MPA view transitions work in Chrome + Safari. So the play is *restraint + one or two signature moments*, not a JS animation framework bolted onto every section.

---

## 2. Stack research (primary sources)

### 2.1 Frameworks

| Framework | Current | Portfolio verdict |
|---|---|---|
| **Astro** | 7.4 (Oct 2026); 7.0 brought Vite 8 + Rust compiler ([astro.build/blog](https://astro.build/blog/)) | Best default. Zero-JS static output, typed Content Collections with Zod schemas ([docs](https://docs.astro.build/en/guides/content-collections/)), `<ClientRouter />` view transitions with `fallback="animate"` simulation ([docs](https://docs.astro.build/en/guides/view-transitions/)) |
| **Next.js** | 16.3–16.4; Cache Components finish PPR, Turbopack default, React 19.2 View Transitions ([nextjs.org/blog/next-16](https://nextjs.org/blog/next-16)) | Pick if you want React end-to-end (next/font, next/og, R3F everywhere). Heavier baseline than a portfolio needs |
| **SvelteKit** | 3.0 (Oct 1, 2026) ([svelte.dev/blog/sveltekit-3-is-here](https://svelte.dev/blog/sveltekit-3-is-here)) | Svelte 5 runes + built-in `transition:` directives; smaller ecosystem, fresh 3.0 major |
| **RRv7 / TanStack Start** | TanStack Start 1.0 (Oct 2026) ([tanstack.com/blog/tanstack-start-1-0](https://tanstack.com/blog/tanstack-start-1-0)) | App-like portfolios only — overkill for a content site |

### 2.2 Animation stack — the 2026 layer model

1. **CSS-native (default layer, no JS):**
   - **Scroll-driven animations** `animation-timeline: view()/scroll()` — cross-browser (Chrome 115+, Firefox 134, Safari 18.2+; Safari lacks compositor threading so keep effects cheap) ([caniuse](https://caniuse.com/?search=scroll-driven%20animations)). Wrap in `@supports`; the element must be readable without it.
   - **View Transitions** — same-document is Baseline (Oct 2025) ([web.dev](https://web.dev/blog/same-document-view-transitions-are-now-baseline-newly-available)); cross-document (MPA) works in Chrome 126+/Safari 18.2+, Firefox still flag-only ([caniuse](https://caniuse.com/cross-document-view-transitions), [Chrome docs](https://developer.chrome.com/docs/web-platform/view-transitions/cross-document)). Use `@view-transition { navigation: auto }` for native MPA morphs; Astro's ClientRouter simulates in Firefox.
   - **`@starting-style` + `transition-behavior: allow-discrete`** — Baseline since Aug 2024 (Chrome 117, Safari 17.4/17.5, Firefox 129) ([MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@starting-style)). Pure-CSS entrance animations.
   - **Anchor positioning** — now Baseline; cursor-following labels/tooltips without JS ([web.dev/learn/css/anchor-positioning](https://web.dev/learn/css/anchor-positioning)).
   - **`text-box-trim`** — Chrome 133, Safari 18.2, Firefox 154; headline type aligned to cap-height ([developer.chrome.com](https://developer.chrome.com/blog/css-text-box-trim)).
2. **Motion (motion.dev, ex-Framer Motion)** — v14.6 (Oct 2026); 14.2 made `x/y/scale` hardware-accelerated; mini `animate` starts ~3.2kb with physics springs; `spring(visualDuration, bounce)` tuning; `scroll()`/`inView` vanilla primitives ([motion.dev/changelog](https://motion.dev/changelog)). Use `motion/react` in React, plain `motion` in Astro islands.
3. **GSAP — 100% free since 3.13** (Apr 2025, Webflow acquisition): SplitText, ScrollTrigger, ScrollSmoother, Flip, DrawSVG all free. SplitText rewritten: 50% smaller, accessible, `autoSplit` responsive re-splitting, mask wrappers ([gsap.com/blog/3-13](https://gsap.com/blog/3-13)). Best for one signature kinetic-type or scrubbed section.
4. **Lenis** smooth scroll — now `lenis` (1.3.x) by darkroom.engineering; `@studio-freight/lenis` deprecated ([lenis.dev](https://lenis.dev)). Do **not** combine with GSAP ScrollSmoother (pick one). Caution: Lenis can interfere with native CSS scroll timelines — test, or drive effects from Lenis progress.
5. **WebGL (optional, one moment)** — three.js r186, R3F v9 stable ([r3f migration guide](https://r3f.docs.pmnd.rs/tutorials/v9-migration-guide)); **OGL** is the light option for hand-written shaders ([oframe.github.io/ogl](https://oframe.github.io)). Lazy-load below the fold.

### 2.3 Fonts, hosting, content

- **Fonts:** self-host via Fontsource (Astro) or `next/font` (Next); variable fonts; web.dev's [font best practices](https://web.dev/articles/font-best-practices).
- **Hosting:** Cloudflare Pages = unlimited bandwidth/requests on free tier ([limits](https://developers.cloudflare.com/pages/platform/limits/)); Vercel Hobby free, pauses instead of billing ([docs](https://vercel.com/docs/plans/hobby)); Netlify moved to credit-based free (300 credits/mo) ([pricing](https://www.netlify.com/pricing)); GitHub Pages soft 100GB/mo ([limits](https://docs.github.com/en/pages/getting-started-with-github-pages/github-pages-limits)). Static portfolio → Cloudflare or GitHub Pages; Next.js → Vercel.
- **Content:** Astro Content Collections + MDX for blog/projects; Keystatic if you want a git-based CMS UI ([docs](https://docs.astro.build/en/guides/cms/keystatic)); OG images at build time with Satori ([satori](https://github.com/vercel/satori)); `@astrojs/sitemap` + `@astrojs/rss` for the basics.

---

## 3. The Emil Kowalski system (what we exhausted)

### 3.1 What's installed locally — and what it encodes

The installed skill set **is** the official Emil Kowalski skill suite he describes building in ["Agents with Taste"](https://emilkowal.ski/ui/agents-with-taste) ("If you know what great feels like, describe the rules, then give them to your agents so they can follow them") — plus his aiforui.dev-adjacent skills. Concretely:

| Skill | What it encodes |
|---|---|
| `emil-design-eng` | The master philosophy: taste is trained, unseen details compound, beauty is leverage. The animation decision framework (frequency → purpose → easing → speed), Sonner principles, the Before/After/Why review table format |
| `animate` (+ `RECIPES.md`) | Construction skill. Runs the gate in order, then builds from 12 ready recipes: button press, dropdown, tooltip (`data-instant` skip), modal, drawer (`translateY(100%)` + `--ease-drawer`), toast (`@starting-style`, `ease` — Sonner's personality), accordion, stagger, hold-to-confirm, clip-path tab indicator, scroll reveal, drag-to-dismiss (velocity > 0.11, pointer capture, damping, multi-touch protection) |
| `review-animations` | The gatekeeper: 10 non-negotiable standards, "default to flagging; approval is earned", Block/Approve verdict. Remedial hierarchy starts at **delete the animation** |
| `improve-animations` | Audit-then-plan: surveys the whole codebase's motion, writes self-contained plans with exact values for a zero-context executor |
| `find-animation-opportunities` | The restraint filter: sweeps for genuine opportunities, **rejects most candidates**, requires a "rejected candidates" section. Hard cap of 5–7 suggestions per app |
| `ui-animation` | The deep reference library: 18 reference files (spring physics, gesture/drag, scroll, SVG, choreography, curve-fitting from screen recordings, interface SFX, symptom-indexed debugging: "sluggish, robotic, cheap, jumpy") |
| `animation-vocabulary` | 100+ named effects for precise prompting ("rubber-banding", "origin-aware", "container morph") |
| `apple-design` | WWDC *Designing Fluid Interfaces* translated to web: springs with damping/response, velocity handoff, momentum projection formula, rubber-banding function, materials (`backdrop-filter`), size-specific tracking |
| `good-css` | Modern-CSS craft: logical properties, `oklch()`, `clamp()` tokens, hover gating, `overflow: clip` |
| `frontend-design` | Anti-"AI slop" design direction: the 5 clusters of generated-design tells, "spend your boldness in one place", two-pass plan-then-critique process |
| `web-design-guidelines` | Vercel's Web Interface Guidelines audit (fetches latest rules from [vercel-labs/web-interface-guidelines](https://github.com/vercel-labs/web-interface-guidelines)) |

### 3.2 The core rules (the taste engine, distilled)

These are the load-bearing rules every portfolio decision should pass through:

1. **The frequency gate.** 100+/day (keyboard, command palette): **no animation, ever** — Raycast is the correct model. Tens/day: near-imperceptible or nothing. Occasional (modals, drawers, toasts): standard. Rare/first-time: this is where the delight budget lives.
2. **Purpose must be nameable:** feedback, spatial consistency, state indication, preventing a jarring change, explanation, or delight (rare tier only). "It looks cool" on a frequently-seen element is a reason to stop.
3. **Speed = respect.** UI stays under 300ms (press 100–160ms, tooltips 125–200ms, dropdowns 150–250ms, modals/drawers 200–500ms). Never `ease-in` on UI — it delays the exact moment the user watches. Custom curves only: `--ease-out: cubic-bezier(0.23, 1, 0.32, 1)`, `--ease-in-out: cubic-bezier(0.77, 0, 0.175, 1)`, `--ease-drawer: cubic-bezier(0.32, 0.72, 0, 1)`.
4. **Physical correctness.** Never `scale(0)` (start `0.9–0.97` + opacity — "nothing appears from nothing"); popovers scale from their trigger (`transform-origin`), modals stay centered; exits are faster than entrances; exit the way you entered (symmetric paths).
5. **Interruptibility.** Transitions (not keyframes) for rapidly-triggered UI; springs for gestures, carrying velocity through interruption; animate from the live value, never the target.
6. **Performance:** `transform`/`opacity` only; CSS beats JS under load (main thread); WAAPI for programmatic-without-dependency; pause loops off-screen; `will-change` only during motion.
7. **Cohesion:** motion matches the component's personality (Sonner uses `ease` and is slightly slower on purpose). Asymmetric timing: slow where the user decides, snappy where the system responds.
8. **Scroll discipline** (from `ui-animation/references/scroll-animations.md`): above-the-fold never scroll-reveals; reveal only 2–4 chosen moments, once, never on scroll-up; scrubbed motion has **no duration and no easing** (scroll position is the clock); parallax ≤ 15% differential, skip on mobile; never hijack scroll.

### 3.3 From the web: his blog, courses, and his own site

- **["You Don't Need Animations"](https://emilkowal.ski/ui/you-dont-need-animations)** — "the best animation is no animation"; keyboard-initiated actions never animate; 180ms dropdown beats 400ms; instant tooltips after the first.
- **["Developing Taste"](https://emilkowal.ski/ui/developing-taste)** — taste is "a trained instinct", not preference; surround yourself with great work, rationalize *why* things feel good, practice. "In a world of abundance, we treasure taste."
- **["Agents with Taste"](https://emilkowal.ski/ui/agents-with-taste)** — "almost every 'taste' decision has a logical reason if you look close enough"; write the rules down as skills, be strict (deterministic rules + flowcharts), feed them to agents. Directly validates this build approach.
- **["7 Practical Animation Tips"](https://emilkowal.ski/ui/7-practical-animation-tips)** — scale(0.97) press, no scale(0), `data-instant` tooltips, custom easings ([easings.co](https://easings.co)), origin-aware popovers, <300ms, blur(2px) to save an unsavable crossfade.
- **["The Magic of Clip Path"](https://emilkowal.ski/ui/the-magic-of-clip-path)** and **["Building a Toast Component"](https://emilkowal.ski/ui/building-a-toast-component)** / **["Building a Drawer Component"](https://emilkowal.ski/ui/building-a-drawer-component)** — the engineering stories behind Sonner/Vaul's invisible details.
- **[animations.dev](https://animations.dev)** — his course: Animation Theory, CSS Animations, Motion, Good vs Great (orchestration, accessibility, performance), plus walkthroughs rebuilding **Family's drawer, Dynamic Island, a navigation menu, SVG animations**. Enrollment closed at time of research; reopens Nov 10. Includes **15 AI skills**.
- **[aiforui.dev](https://aiforui.dev)** — his newer course on building UI with AI: **28 named skills** (`/prototype`, `/design-foundations`, `/typography`, `/color`, `/ui-polish`, `/animations`, `/ui-review`, `/prep-for-prod`, `/unslop-design`, `/unslop-code`, `/touch-and-accessibility`, `/ask-emil`, …), a **244-term visual vocabulary**, and **Lapse** (a tool for slowing down and reviewing animations). The local `animate`/`review-animations`/`animation-vocabulary`/`unslop` skills overlap heavily with this — most of the paid curriculum's machinery is already installed.
- **His own site ([emilkowal.ski](https://emilkowal.ski))** — the design statement: typography-only, single narrow column, no images, no chrome, bold titles with one-line descriptions. The portfolio is a well-typeset index.

### 3.4 The pattern across the best design-engineer sites

Checked [emilkowal.ski](https://emilkowal.ski), [paco.me](https://paco.me) (Paco Coursey — cmdk, next-themes, Linear/Vercel) and [rauno.me](https://rauno.me) (Rauno Freiberg — Devouring Details, Vercel):

- **Text-first index, not a card gallery.** No thumbnails, no metrics, no grid of project cards. Bold link + one-line description.
- **The site is the demo of restraint.** paco.me: "the absence of decoration is the design statement." rauno.me opens with a manifesto ("Make it fast. Make it timeless. Make it.").
- **One tiny signature interaction** instead of ten effects: rauno's click-to-copy email that confirms in place; cmdk's ⌘K heritage.
- **Voice-driven copy.** Terse, specific, human ("Pray at the altar of hard work").

---

## 4. Unique-but-buildable ideas, ranked (uniqueness × feasibility)

The thesis the research supports: **uniqueness in 2026 comes from doing the common things uncommonly well, plus one or two signature moments** — not from maximum effects. Each idea maps to a technique and a skill that will build it correctly.

| # | Idea | Why it's unique | How (technique + source) | Effort |
|---|---|---|---|---|
| 1 | **View-transition morphs everywhere** — project card *becomes* the project page (shared-element transition); `@view-transition { navigation: auto }` for MPA morphs with instant-navigation fallback | Most portfolios have hard page loads or SPA fades; native morphs read as magic and are now 10 lines of CSS | Native MPA view transitions (Chrome/Safari) or Astro `<ClientRouter fallback="animate">`; `transition:name` on cards/avatars/headings | S–M |
| 2 | **The restraint-forward index** — typography-only homepage in the emilkowal.ski/paco.me lineage: no cards, no thumbnails, bold link + one-line description, one orchestrated page-load stagger (30–80ms) and nothing else moving | Reads as taste precisely because everything else is quiet; the anti-AI-slop look is itself distinctive now | `frontend-design` skill's anti-defaults + stagger recipe; `text-box-trim` for headline alignment | S |
| 3 | **A drag-physics project drawer** — project details open in a bottom sheet you can flick away (velocity dismissal, rubber-band at the top, spring settle) | Almost nobody ports Vaul-grade physics to a portfolio; it demos interaction-engineering skill *in the artifact itself* | `animate/RECIPES.md` drag-to-dismiss (velocity > 0.11, pointer capture, damping) + apple-design spring values (`duration 0.5, bounce 0.2`) | M |
| 4 | **One signature kinetic-type moment** — hero headline reveals with free GSAP SplitText masked line reveals, staggered like a wave (heading leads, support follows), `autoSplit` for responsive re-split | Free SplitText is brand new (Apr 2025); done once and well it's the memorable thing | GSAP 3.13 SplitText + ScrollTrigger; `frontend-design` "spend your boldness in one place" | S–M |
| 5 | **CSS-native scroll craft** — one scrubbed sequence max (e.g. a sticky 2–3-viewport story of how a project was built), scroll-driven `animation-timeline: view()` reveals on 2–4 chosen moments, `wght` variable-font scrubbing on a display heading | Most portfolios either animate every section or none; doing the *correct* two moments with zero-JS CSS is both rare and fast | `scroll-animations.md` rules + `@supports` wrappers; no duration/easing on scrubbed motion | S |
| 6 | **⌘K command palette — with no open animation** | The Emil twist: keyboard-initiated = instant (Raycast model). Most portfolios' palettes animate; the instant one *feels* professional. Doubles as the a11y nav | cmdk (paco.me's library) or Base UI Menu; gate: no animation ever | S |
| 7 | **Confirm-in-place micro-interactions** — copy-email button becomes "Copied" in place (never a corner toast); hold-to-confirm on the contact form's send; clip-path fill is linear, release snaps at 200ms | "Confirm in place, not in a corner" is a signature Emil principle almost nobody implements | `animate/RECIPES.md` hold-to-confirm (clip-path inset), `ui-animation` contextual-animations | S |
| 8 | **Spring cursor / magnetic hover on exactly one element** — hero avatar or the primary CTA tracks the mouse through `useSpring` (stiffness ~100, damping ~10), gated behind `@media (hover: hover) and (pointer: fine)` | The canonical Emil example of decoration in the right place: marketing page yes, functional UI no | `emil-design-eng` spring-mouse section; Motion `useSpring` | S |
| 9 | **Sparse sound design** — gesture-unlocked, one-shot SFX on rare moments only: theme toggle, message sent, copy. Respect mute + `prefers-reduced-motion` | Sound is the most underused dimension on the web; done sparsely it's memorable, done lavishly it's a mistake | `ui-animation/references/interface-sfx.md` (AudioContext unlock, gain ~0.1–0.2, tens-of-ms clips) | S |
| 10 | **A "playground" page** — a small island where visitors can actually play with your components/animation settings (live easing-curve editor, drawer with adjustable bounce) — the Sonner principle: "let people touch the product" | Turns a portfolio into a product; directly demonstrates what you claim | Sonner principles in `emil-design-eng`; Motion Studio (MCP + visual spring editing, Oct 2026) as tooling | M–L |
| 11 | **One lazy-loaded WebGL moment** — a single OGL shader plane (ASCII/monochrome treatment on a hero image) that only loads below the fold or on intent | One wow moment beats a 3D site; OGL keeps it tiny | OGL (v1.0.11) hand-written shader; lazy-load | M–L |
| 12 | **Publish the craft** — ship the repo with a `DESIGN.md` (design tokens + motion rules, per the `impeccable` skill), and optionally an /ask-my-site AI index of your writing | The repo itself becomes proof of process; "agents with taste" as portfolio artifact | `impeccable` `document`/`init` commands | S |

### What to deliberately avoid (the 2026 cliché list)

From the `frontend-design` skill's generated-design tells + the Emil suite's anti-patterns:

- Warm-cream + serif + terracotta (#D97757-ish) default; near-black + acid-green/vermilion accent; broadsheet hairline layout; the SaaS card kit (one radius, one soft shadow, gradient washes); tracked ALL-CAPS eyebrow labels; `A · B · C` meta strings; `→` appended to every link; numbered 01/02/03 markers on non-sequences.
- Fade-and-slide-up entrance on every section; hover transition on every card; scroll-reveal above the fold; re-animating on scroll-up; parallax > 15%; scroll-jacking; Lenis + ScrollSmoother together.
- `transition: all`, `ease-in`, `scale(0)`, keyframes on rapidly-triggered elements, hover effects ungated on touch, motion without `prefers-reduced-motion` handling.

---

## 5. Recommended build plan

**Stack:** Astro 7 + Content Collections (MDX) + Fontsource variable fonts + Cloudflare Pages. Animation: CSS-native core (view transitions, `animation-timeline`, `@starting-style`, `text-box-trim`) + `motion` vanilla in islands + free GSAP SplitText for the one signature type moment + Lenis only if the scrubbed section needs it. One OGL moment if wanted, lazy-loaded. OG images at build time (Satori), sitemap + RSS.

**Process (this is the leverage of the installed skills):**
1. **Direction first:** `frontend-design` two-pass plan (tokens → critique against the AI-tells list → build). Optionally `ui-ux-pro-max --design-system` for palette/typography starting points, then deviate deliberately.
2. **Motion with the gate:** every effect passes the frequency/purpose/speed gate (`animate` skill), sourced from `RECIPES.md` values, never invented.
3. **Sweep before shipping:** `find-animation-opportunities` for what should animate; `review-animations` on the final diff (Before/After/Why table, Block/Approve); `web-design-guidelines` for the Vercel interface audit; `impeccable polish`/`critique` for the final pass.
4. **Feel-check:** slow-motion + frame-by-frame in DevTools (or Lapse), fresh-eyes review the next day, real device for gestures.

**Pick 3–4 of the ranked ideas, not 12.** The research is unanimous on this point: the best sites in this genre (emilkowal.ski, paco.me, rauno.me) are restraint-forward with one or two signature moments. A sensible default set: #1 (view-transition morphs) + #2 (typographic index) + #4 (SplitText hero, once) + #7 (confirm-in-place micro-interactions), then add #3 or #10 if you want a bigger signature.
