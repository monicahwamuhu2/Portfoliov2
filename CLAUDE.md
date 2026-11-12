# CLAUDE.md — Monicah's Portfolio

## Who this site is for
The personal portfolio of **Monicah**, a full-stack software engineer based in Nairobi, Kenya, focused on **AI engineering** (LLM apps, RAG, intent classification, AI tooling). The site should feel like her: creative, colorful, confident and playful, while staying professional for recruiters and clients.

Audiences, in priority order:
1. Recruiters and hiring managers. They skim in 30–60 seconds and must find who she is, what she builds and how to reach her immediately.
2. Engineering leads, who open case studies and check technical depth.
3. Potential clients and collaborators.

**Core rule:** the experience wraps the content and never blocks it. All content must be reachable and readable without waiting for an animation, and with motion disabled.

## Working protocol (read before every phase)
- Work in the phases defined in the kickoff prompts. Finish, verify and summarize each phase before starting the next.
- At every 🛑 approval gate, present your findings or proposals, then **stop and wait for explicit approval**. Do not generate assets, install new dependencies or start building past a gate on your own.
- If a tool, model, library or API you planned to use is unavailable or behaves differently than expected, **say so and propose an alternative before substituting it.**
- Verify APIs against current documentation before using them. Never invent library behavior.
- Never claim something works without verifying it (build, lint, browser check, tests). State exactly what you verified.
- Explain significant architectural decisions and trade-offs in your summaries. The owner wants to understand the why, not only the result.

## Reference inputs (`/reference`)
| File / folder | What it is | How to use it |
|---|---|---|
| `reference-sites.md` | URLs of sites whose motion or layout the owner likes, each with a note on what she likes | Explore each with the browser tools. Study scroll choreography, pinned sections, zoom effects, transitions, cursor behavior and how sections enter and exit. |
| `walkthroughs/*.mp4` | Screen recordings of reference sites, optionally with voice notes | Extract frames with ffmpeg and inspect them. Transcribe spoken notes with local Whisper. Look closely at frames around important transitions. |
| `moodboard/*` | Images, colors, textures and type samples she likes | Use for visual direction. |
| `photos/*` | Her own photos (portrait, workspace, Nairobi, events) | Preferred source for real imagery. |
| `dislikes.md` | Things she does not want (fonts, effects, layouts) | Treat as hard constraints. |

**Borrow techniques, never designs.** Reference sites are for learning how an interaction works: timing, easing, pinning, reveal patterns. Do not copy their layouts, copy, imagery, branding or code. The result must be original and recognizably hers.

## Design direction
- **Personality:** bold, warm, colorful, curious, slightly playful. "Creative technologist," not "corporate template."
- **Palette:** vibrant and warm, inspired by Nairobi sunsets and East African beadwork. Deep indigo/near-black base with saturated accents (sunset orange, magenta, electric teal, gold). Define colors as tokens. Check every text/background pair against WCAG AA contrast.
- **Themes:** dark (default) and light, plus optional 2–3 playful mood palettes.
- **Typography:** create an original direction. Propose 2–3 pairings (expressive display + highly readable body + monospace), using free fonts only (Google Fonts, Fontshare or other OFL-licensed fonts). Self-host with `next/font`. Respect anything in `dislikes.md`.
- **Motion language:** smooth, springy, intentional. Motion should explain or reward, never distract. Durations and easings are defined once as tokens.
- **Avoid:** generic template looks, gradients on everything, walls of skill badges, stock-illustration clichés, autoplaying sound.

## Signature cinematic sequence: "The Window"
One pinned, scroll-driven sequence is the centerpiece of the landing page, like a camera move in a film:
1. **Hero:** a stylized Nairobi skyline at golden hour, drawn in code (SVG and/or a shader). Particles drift like dust in sunset light. Name, role and CTAs sit on top and render instantly.
2. **Zoom through:** as the visitor scrolls, the camera pushes into one lit window in the skyline. The window grows until it fills the screen.
3. **Inside:** the window becomes a screen in her workspace. Code types itself, then dissolves into a node graph.
4. **The AI pipeline:** the nodes rearrange into an animated RAG pipeline (query → embed → retrieve → generate). This lands the "AI engineer" message.
5. **Exit:** the pipeline's output becomes the first featured project card, handing off into the Featured Work section.

The sequence must work in reverse when scrolling up, collapse to a simple static layout under `prefers-reduced-motion`, and simplify on mobile. Propose refinements or an alternative at the visual-direction gate; this is the default concept.

## Asset strategy (free only)
Order of preference:
1. **Built in code:** SVG, CSS, Canvas 2D, GLSL shaders via React Three Fiber. Use for the skyline, window zoom, particles, node graphs and backgrounds.
2. **Her own photos** from `/reference/photos`, optimized with `next/image`.
3. **Free 3D:** Blender, only if a 3D object is truly needed. Export as compressed glTF (Draco/meshopt) or render to an image sequence.
4. **Royalty-free stock** (Unsplash, Pexels) only if needed, with each source and license recorded in `/content/credits.md`.
5. **AI image tools the owner already has**, only if she asks. Do not assume a paid generation service or MCP is available.

**Video:** avoid unless it clearly earns its weight. If used, prefer short, muted, compressed WebM/MP4 with a poster image, or a canvas-drawn image sequence for scroll-scrubbed playback. Explain the trade-off (file size vs. smoothness vs. scrub accuracy) before choosing.

## Tech stack
Use latest stable versions and verify APIs against current docs.

| Concern | Choice | Why |
|---|---|---|
| Framework | Next.js (App Router) + TypeScript | Static pages for speed and SEO, plus server routes for the AI chat |
| Styling | Tailwind CSS | Fast iteration, design tokens in one theme |
| UI animation | Motion | Component and layout animations, gestures, shared-element transitions |
| Scroll choreography | GSAP + ScrollTrigger | Pinned, scrubbed, timeline-based scroll sequences; powers "The Window" |
| Smooth scroll | Lenis | Integrates with ScrollTrigger; disabled under reduced motion |
| 3D / shaders | React Three Fiber + drei | Hero visual; lazy-loaded |
| Content | MDX under `/content` | Version-controlled case studies and writing |
| AI chat | Vercel AI SDK + Claude API | Streaming chat grounded in portfolio content |
| Rate limiting | Upstash Redis (free tier) | Protects the paid AI endpoint |
| Command palette | cmdk | ⌘K navigation |
| Analytics | Vercel Analytics (free tier) | Lightweight |
| Testing | Playwright (e2e, scroll tests, video recording, axe accessibility) + Vitest | Covers behavior, scroll robustness and accessibility |
| Deploy | Vercel (Hobby tier) | First-class Next.js hosting |

Do not add dependencies beyond this list without stating why and asking first. The only running cost should be Claude API usage for the chat, protected by rate limits and a spending cap.

## Site map
- `/` — Landing (scroll journey)
- `/work` — Projects, filterable (AI, Full-stack, Frontend, Design)
- `/work/[slug]` — MDX case studies
- `/lab` — Live AI demos and experiments
- `/about` — Story, values, timeline, achievements, currently learning
- `/writing` — MDX blog/notes
- `/resume` — Web resume + PDF download
- `/uses` — Tools and setup (optional)
- A playful, on-brand `404`

## Landing page sections (scroll order)
1. **Hero + "The Window" sequence**
2. **Intro / Story** — 2–3 sentences revealed word by word as you scroll
3. **Featured Work** — 3–4 flagship projects as large visual cards. Hover previews; click triggers a shared-element transition into the case study.
4. **Skills constellation** — interactive node graph grouped by domain. Hovering a skill highlights the projects that used it.
5. **Experience timeline** — scroll-animated
6. **Achievements** — animated counters/cards
7. **Ask my AI** — embedded chat preview with suggested question chips
8. **Contact** — big, colorful closing section with a memorable final line

## Signature features
1. **"Ask Monicah's AI"** — streaming assistant grounded only in portfolio content via retrieval. Cites source pages, declines off-topic requests, rate limited.
2. **"The Window"** cinematic scroll sequence.
3. **Shared-element page transitions** (Motion and/or the View Transitions API).
4. **Section enter/exit choreography** — every section has a deliberate entrance and exit, not just fade-up.
5. **⌘K command palette** — navigate, switch theme, copy email, open the AI chat.
6. **Mood/palette switcher.**
7. **Custom cursor** on fine pointers only, morphing over interactive elements with context labels ("View", "Drag", "Open").
8. **Live "currently building" / GitHub activity card.**
9. **One easter egg**, e.g. a hidden terminal mode.

## Non-negotiable engineering rules
- **Accessibility:** semantic HTML, full keyboard navigation, visible focus states, alt text, WCAG AA contrast. Under `prefers-reduced-motion`, disable smooth scroll, pinning, parallax, zoom and 3D motion and show static equivalents. The custom cursor never hides the system cursor's function.
- **Performance budget:** Lighthouse ≥ 90 on mobile (Performance, Accessibility, Best Practices, SEO). LCP < 2.5 s, CLS < 0.1. Lazy-load 3D and heavy libraries; never block first paint on WebGL. Pause animations and the canvas when off-screen or when the tab is hidden. Static fallback when WebGL is unavailable or the device is low-powered.
- **Scroll robustness:** every scroll sequence must behave correctly scrolling down and back up, under fast scrolling, after a window resize, on refresh mid-page, and after navigating away and back. Kill and recreate ScrollTrigger instances on route changes; call `ScrollTrigger.refresh()` after layout-affecting loads (fonts, images).
- **Mobile first:** design for phones first. Simplify or replace heavy effects on touch devices.
- **Content separate from code:** all copy and data live in typed files or MDX under `/content`.
- **AI endpoint security:** server-only keys, input length limits, per-IP rate limiting, a constraining system prompt, minimal logging, graceful error and rate-limit UI states.
- **SEO:** per-page metadata, OG images via `next/og`, sitemap, robots.txt, JSON-LD `Person` schema.
- **Code quality:** strict TypeScript, small focused components, no premature abstraction, ESLint + Prettier.

## Content (fill in)
- Full name: [[FULL NAME]]
- Positioning line: [[e.g. "Full-stack engineer building thoughtful products with AI"]]
- Short bio (3 sentences): [[BIO]]
- Email / LinkedIn / GitHub / X: [[LINKS]]
- Flagship projects (name, problem, your role, stack, outcome/metric, links, screenshots):
  - [[AI project — e.g. RAG-based LLM application]]
  - [[AI project — e.g. intent classification system]]
  - [[Full-stack project — e.g. M-Pesa / Daraja STK Push integration]]
  - [[Product — e.g. logistics app]]
- Experience: [[roles, organizations, dates, 1–2 impact bullets each]]
- Education: [[degree, university, dates]]
- Achievements: [[e.g. IEEE Xtreme national ranking]], [[certifications]]
- Currently learning: [[TOPICS]]
