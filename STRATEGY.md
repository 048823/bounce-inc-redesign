# BOUNCE Inc — Premium Immersive Website Redesign
### Creative & Technical Master Plan · WeBuild Creative Studio (CDO)

> Scope note: this document is the implementation-ready **direction + system**. A working, on-brand homepage (`index.html`) ships alongside it as the reference build for the recommended concept. Full multi-page production (16 core pages, cinematic media, CMS) is broken into delegated squad workstreams — see `§ Production Plan`.

---

## Phase 1 — Audit (current bounceinc.com.au)

**Strengths**
- Strong, ownable brand energy ("awesome", "next level fun") and a real product people love.
- Deep venue footprint: **26 venues across 7 states/territories** — a genuine discovery asset.
- Clear commercial lines already defined: Open Jump, Parties (3 tiers), miniBOUNCE, Freestyle Academy squads, Memberships, Holiday Camps, Gift Cards.
- Authentic footage exists (YouTube: BounceTrampolinePark) — real athletes, real venues.

**Weaknesses / conversion problems**
1. **Redundant, overloaded navigation** — multiple menus create cognitive load; no single clear path from "I want to jump" → booking.
2. **Placeholder / Lorem Ipsum content** live on the site (Free-Jump Arena) — reads as unfinished, erodes premium trust.
3. **Venue discovery is weak** — the single biggest missed opportunity. No map, no search, no "near me". 26 venues but no fast path to the right one.
4. **Pricing hidden** — no at-a-glance pricing on the homepage; forces navigation before intent is captured.
5. **"Book Now" repeated 4+ times with no venue context** — booking friction, ambiguity about *where* you're booking.
6. **Generic template feel** — doesn't match the energy of the physical product; no cinematic use of the footage that already exists.

**Reusable assets:** logo/wordmark, feature icons, party hero imagery, YouTube action footage (re-edit/re-grade), venue data, membership/party taxonomy. **Replace:** placeholder blocks, low-res promo banners, any stock-feel imagery.

---

## Phase 2 — Strategy

**Business goals (priority order):** venue bookings → party/event enquiries → membership sign-ups → school/corporate leads → repeat visits → brand awareness.

**Primary success metric:** venue-finder → booking start rate. **Supporting:** enquiry form completions, membership CTA clicks, Core Web Vitals pass rate, mobile bounce rate.

**Audiences & message hierarchy**
| Audience | Core need | Lead message |
|---|---|---|
| Kids / teens | "Is this exciting?" | Big air, freedom, your crew |
| Parents / families | "Is it safe & easy?" | Big air, safe landings — booked in 60s |
| Birthday planners | "Take the stress off me" | We host, you relax |
| Schools | "Curriculum-safe, low admin" | Excursions + risk docs sorted |
| Corporate | "Team day people want" | Private courts, catering, chaos optional |
| Members | "More value, more often" | Priority booking + member pricing |

**Positioning:** *Australia's home of freestyle movement* — an interactive brand campaign, not a venue listing.

**Sitemap (16 core pages):** Home · Venues index · Venue detail (×26, templated) · Activities · Parties · School groups · Corporate/groups · Memberships · Gift cards · Safety · About · FAQ · Contact · Booking entry · Campaign landing · Error/empty states.

**Key user journeys:** (1) Parent: Home → Venue finder → Venue → Book. (2) Teen: Social → Campaign landing → Activities → Book. (3) Planner: Home → Parties → Venue availability → Enquire. (4) School: Groups → Enquiry form.

---

## Phase 3 — Creative Concept (3 directions)

### Direction A — **"Suspended"** ✅ RECOMMENDED
- **Visual language:** Ink-black canvas, electric-lime signature, action-pink CTAs, cyan accents. Athletes frozen mid-flight against depth-layered glow fields and a faint kinetic grid.
- **Hero:** Full-bleed cinematic figure suspended mid-flip; oversized "GET SOME AIR" with outline/fill typographic play; dual CTA (Book / Find a venue).
- **Motion:** Weightlessness — slow float, parallax depth, momentum-based reveals. Motion *supports* narrative, never decorates.
- **Type:** Anton (display, condensed impact) + Archivo Black (headings) + Inter (UI/body).
- **Colour:** `#0A0A0B` ink · `#B4FF3D` volt · `#FF2E7E` magenta · `#19E6C9` cyan · `#F5F4EF` paper.
- **Strengths:** premium + energetic, showcases real footage, huge contrast for CTAs, ownable. **Risks:** dark UI needs contrast discipline. **Best use:** captures all audiences; parents still get clarity.

### Direction B — **"Playbook"**
- Bright, daylight, oversized rounded shapes, sticker/collage energy, primary-colour blocks.
- Hero: bold flat illustration + real photo cutouts. Motion: bouncy, spring physics.
- **Strengths:** friendly, kid-first, cheap to produce. **Risks:** reads younger/less premium, closer to generic category. **Best use:** if brand skewed hard to under-8s.

### Direction C — **"Broadcast"**
- Sports-broadcast language: score bugs, telemetry overlays, replay framing, high-contrast editorial.
- Hero: motion-tracked footage with data overlays. **Strengths:** teen/competitive appeal, dynamic. **Risks:** can feel cold to parents, heavier build, overlay legibility. **Best use:** Freestyle Academy / competitive sub-brand.

**Recommendation: Direction A — "Suspended".** It is the only direction that is simultaneously premium, energetic, parent-trustworthy and teen-exciting, and it makes the *existing authentic footage* the hero rather than fighting it. B skews too young for a $10k+ premium bar; C alienates the paying parent. A is delivered as the reference build in `index.html`.

---

## Phase 4 — Experience Design (homepage, in build order)

1. **Immersive hero** — cinematic figure + glow/grid depth, headline, dual CTA, live stat row, scroll cue. Fully usable with motion off.
2. **Kinetic marquee** — signature activities scroll band (decorative, `aria-hidden`).
3. **Venue finder** *(hero feature)* — instant search + state filter chips + card grid + live count/empty state. Fastest possible discovery→book path.
4. **Signature experiences** — 4-card energy grid (Open Jump, Dodgeball, Ninja/Wall, miniBOUNCE).
5. **Inside Bounce** — 3-scene scroll story (venue reveal → airbag flip → academy), alternating layout.
6. **Audience split** — Parties / Schools & Camps / Corporate, each with its own message + CTA.
7. **Safety & parent reassurance** — trust grid (marshals, graded zones, daily checks, grip socks).
8. **Membership** — high-contrast offer block with anchor price.
9. **Final CTA** — oversized "Ready to fly?" + primary booking action.
10. **Footer** — explore / support / company / socials + newsletter.

**Venue detail template (×26):** hero gallery, activities available, hours, pricing guidance, upcoming sessions, party availability, accessibility + safety, map, sticky book bar. Locally relevant, globally consistent.

---

## Phase 5 — Motion & 3D Plan (Fable-orchestrated)

| Scene | Trigger | Duration | Camera / Layer | Mobile | Reduced-motion fallback | Perf |
|---|---|---|---|---|---|---|
| Hero float | Load | Ambient loop | Parallax glow + subtle figure drift | Static figure, no drift | Static composition | CSS-only, no JS raf |
| Marquee | In view | Loop 26s | Horizontal translate | Slower/shorter | Paused static band | GPU transform |
| Section reveals | IntersectionObserver | 0.7s | translateY + fade | Same, lighter | Instant show (no transform) | Observer, unobserve after |
| Inside-Bounce story | Scroll into row | per-row | Alternating slide + media reveal | Stacked, fade only | Static stacked | No pin on mobile |
| Airbag flip (scene 02) | Pin (desktop only) | scroll-scrubbed | Slow-mo footage scrub | No pin — plays inline | Poster frame | Lazy video, `preload=none` |
| CTA glow | In view | 0.7s | Radial glow fade-in | Same | Static | Blurred gradient, no anim |

**Rule:** section pinning only on the airbag scene, desktop only, where it improves storytelling. No motion during transactional tasks (finder/booking). Every sequence validated for narrative clarity, hierarchy, performance, mobile, accessibility, conversion. 3D (Three.js) reserved for the airbag scene only, lazy-loaded, with a static poster fallback — used only where it adds emotional value.

---

## Phase 6 — Visual Production (Higgsfield prompt briefs)

Prioritise real BOUNCE footage; generate only to fill gaps, matching real venues/uniforms/equipment. **No misleading representation of activities or safety.**

- **HERO (image→video):** *"Cinematic wide shot, young athlete suspended mid-backflip at apex over a trampoline park, volumetric magenta + cyan stage lighting, deep shadow, shallow depth of field, motion energy, premium sports-brand campaign, dark background, 4K, no text."* Output: 6–8s loop + poster.
- **AIRBAG (scene 02):** *"Slow-motion teenager flipping into a giant airbag, dust of chalk, dramatic side light, freeze at peak, editorial action-sports grade."*
- **VENUE REVEAL (scene 01):** *"Slow parallax push through a vibrant indoor trampoline park, connected courts, warm-cool contrast, people mid-jump, cinematic depth."*
- **miniBOUNCE:** *"Soft, bright, safe toddler play zone, gentle pastel-on-brand lighting, joyful, parents nearby, reassuring."*
- **Treatments for reused YouTube footage:** re-frame to 4:5 + 16:9, colour-grade to brand LUT (crushed blacks, volt/magenta lift), 60fps interpolation, denoise/upscale to 1080p+.

Full asset inventory + per-asset disposition (reuse / re-edit / reframe / upscale / grade / motion / replace / generate) delegated to StudioDesigner (see Production Plan).

---

## Phase 7 — Technical Build

**Reference build (shipped):** single self-contained `index.html` — semantic HTML, design-token CSS, vanilla JS (no dependencies), fonts via Google CDN. Zero build step, instantly deployable, Lighthouse-friendly. Proves Direction A end-to-end.

**Production stack (recommended):** Next.js + TypeScript + React · GSAP + Fable for motion orchestration · Three.js for the single airbag 3D scene (lazy) · Headless CMS (venues + campaigns as content models) · `next/image` + adaptive video · component architecture · analytics + conversion events · SEO metadata + LocalBusiness structured data per venue.

**Design system (in `index.html` as tokens):** colour, type scale, spacing, radius, buttons (primary/volt/ghost), cards, venue module, nav, motion principles, status/empty states, focus styles, responsive breakpoints.

**Content model (CMS):** `Venue{name,state,suburb,geo,hours,activities[],pricing,partyAvailable,accessibility,gallery[],slug}` · `Campaign` · `Activity` · `PartyPackage` · `MembershipTier`.

---

## Phase 8 — Review (against the success standard)

| Criterion | Reference build status |
|---|---|
| Brand impact | ✅ Instantly Bounce — ownable colour + type + energy |
| Conversion | ✅ Venue finder promoted to hero feature; CTA persistent, high-contrast |
| Mobile usability | ✅ Mobile-first, stacked, tap-sized CTAs, no pin on mobile |
| Accessibility | ✅ Skip link, focus rings, `prefers-reduced-motion`, `aria-live` finder, semantic landmarks, keyboard nav |
| Performance | ✅ No framework, no blocking autoplay, observer-based reveals, lazy media slots |
| Originality | ✅ Not a generic trampoline site — campaign-grade |
| Maintainability | ✅ Token-driven, single file; clean upgrade path to Next.js |
| Content scalability | ✅ Venue grid data-driven; CMS model defined |

**Open items before "deployment-ready" sign-off:** real footage/Higgsfield assets, 26 venue detail pages, remaining core pages, CMS wiring, analytics events, full a11y audit on production stack. Tracked in Production Plan.

---

## Production Plan (delegated squad workstreams)

| Owner | Workstream |
|---|---|
| **StudioDesigner** | Asset inventory + per-asset disposition; Higgsfield hero/venue/miniBOUNCE image generation; high-fidelity mobile comps; venue photography treatment. |
| **MotionDesigner** | Cinematic hero loop + airbag scroll sequence via Higgsfield; footage re-grade/reframe/upscale; motion QA per Phase 5 table. |
| **BrandGuardian** | Design-system + brand consistency QA gate; a11y/contrast validation; final sign-off before delivery. |

Sub-issues created under WEB-210. Nothing ships until BrandGuardian approves.
