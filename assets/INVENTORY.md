# Asset Inventory — BOUNCE Inc Redesign (WEB-211)

Audit of `bounceinc.com.au` + YouTube (`BounceTrampolinePark` / "BOUNCE Australia") against the four asset slots in the reference build (`index.html`) and the Phase 6 prompt briefs in `STRATEGY.md`. Disposition legend: **reuse** (ship as-is) · **re-edit** (crop/retouch) · **reframe** (new aspect ratio) · **upscale** (resolution only) · **grade** (colour only) · **motion** (image → video, MotionDesigner) · **replace** (swap subject/venue) · **generate** (no usable source, AI-produced).

## Source audit

| Source | What's there | Usable for redesign? |
|---|---|---|
| bounceinc.com.au homepage | Custom promo banners (Winter Beanie, Friday Night Super Sessions, Parties, Freestyle Academy), activity icon set (hosts, dodgeball, pizza slice, etc.), site logo. Several placeholder `data:image/svg+xml` stand-ins never replaced with real photography. `Free-Jump Arena` section still runs Lorem Ipsum body copy. | Icons + logo: **reuse**. Promo banners: low-res, template-feel — **replace**, don't carry into "Suspended". Placeholder blocks: **replace** (content problem, not an asset). |
| YouTube — BOUNCE Australia (`/c/BounceTrampolinePark`, handle `bouncetrampolinepark`) | Real athlete/venue footage exists per brand (confirmed in `STRATEGY.md` Phase 1 audit); channel is JS-rendered so exact clip titles/timestamps aren't scriptable without the YouTube Data API or manual browse. Footage is standard social-upload quality (not graded, not 60fps, mixed aspect ratios). | **re-edit + reframe + grade + upscale** once MotionDesigner pulls specific clips — reframe to 4:5/16:9, apply brand LUT (crushed blacks, volt/magenta lift), 60fps interpolation, denoise/upscale to 1080p+, per `STRATEGY.md` Phase 6. Flagged here so MotionDesigner doesn't re-audit from zero; exact clip selection needs channel API access (`multica` has no YouTube MCP connected this run). |

## Per-slot disposition (`index.html` asset slots)

| Slot | Location | Brief (Phase 6) | Disposition | Delivered this run |
|---|---|---|---|---|
| Hero — cinematic figure | `.hero-card .figure` (line ~265) | Athlete suspended mid-backflip, magenta/cyan volumetric light, dark bg, 4:5, no text | **generate** — no existing real-footage still matches the exact framing/light needed for a hero-grade static image (source footage is video only, standard grade) | `assets/stills/hero-mid-flip.webp` |
| Scroll Scene 01 — venue reveal | `.story-media` #1 (line ~334) | Slow parallax push through venue, warm-cool contrast, people mid-jump | **generate** for the static comp; MotionDesigner can later **replace** with a real graded parallax clip of an actual venue (e.g. Blacktown NSW, Joondalup WA — both photograph well per venue data in `index.html`) once footage is sourced | `assets/stills/venue-reveal.webp` |
| Scroll Scene 02 — airbag flip | `.story-media` #2 (line ~342) | Slow-mo flip into airbag, chalk dust, coach visible, no misleading safety representation | **generate** — coach + crash mats included in frame per the safety-representation constraint | `assets/stills/airbag-flip.webp` |
| Scroll Scene 03 — Freestyle Academy | `.story-media` #3 (line ~350) | Coach + squad, character motion | **motion** — out of this run's scope (Phase 6 prompt list names hero/airbag/venue/miniBOUNCE only; scene 03 is a MotionDesigner deliverable, not a StudioDesigner still) | Not generated this run — flagged for MotionDesigner |
| miniBOUNCE (Signature Experiences card 04, no dedicated slot yet) | `#experiences` article 4 (line ~320) | Soft, safe toddler zone, pastel-on-brand, reassuring | **generate** — real miniBOUNCE footage exists on YouTube but needs the same re-edit/grade pass as other footage; a still fills the gap now | `assets/stills/minibounce.webp` |

## Generated this run (Higgsfield)

First pass used `cinematic_studio_2_5` — technically clean but off-brief: no lime/magenta stage light, and the venue-reveal draft came back as an athletics indoor track, not a trampoline park. Rejected outright rather than shipped; not agency standard. Re-ran on `flux_2` (pro variant, stronger prompt adherence) with explicit lighting/venue/no-ambiguity language.

BrandGuardian QA gate (round 1) rejected `airbag-flip` and `minibounce` — logged and fixed:
- `airbag-flip` — the coach's shirt carried a logo-like mark; a second regeneration attempt made it worse (a recognisable real "Under Armour" mark, plus the airbag itself regressed to a firm gymnastics crash mattress). Third attempt explicitly specified a plain unbranded top and a soft inflatable PVC airbag (not a mat) — clean.
- `minibounce` — was generic warm mint/teal daycare colour with no lime or magenta, didn't read as BOUNCE. Regenerated with pastel-volt padding + magenta/cyan accent lighting explicitly called out in the prompt.

Final set, all on brand token palette (`--ink #0A0A0B`, `--volt #B4FF3D`, `--magenta #FF2E7E`, `--cyan #19E6C9`):

- `assets/stills/hero-mid-flip.webp` — 3:4, hero card. Dark venue, athlete mid-flip, lime + magenta volumetric beams through haze, deep black bg.
- `assets/stills/venue-reveal.webp` — 16:9, scroll scene 01 (nearest available ratio to the 16:11 slot; crop on implementation). Wall-to-wall in-ground trampoline grid, lime/cyan neon accent strips, people mid-jump.
- `assets/stills/airbag-flip.webp` — 16:9, scroll scene 02. Flip into a soft inflatable airbag, chalk dust, coach in plain unbranded top watching at edge (safety context, no logos), lime/magenta light.
- `assets/stills/minibounce.webp` — 16:9, miniBOUNCE. Pastel-volt padded zone with magenta/cyan accent light, parent close by, safe and joyful.

## High-fidelity mobile comps

`assets/mobile-comps/` — real renders of the actual reference build (`index.html`) at a genuine 390×844 mobile viewport with the four generated stills composited into their slots, proving Direction A holds up on small screens (not mockup illustrations of the UI — the literal shipping code, screenshotted).

BrandGuardian QA gate (round 1) rejected these too: headline/body/CTA were clipped off the right edge on all three. Root cause — Chrome headless's `--window-size` flag has a hard-floor minimum content width (~500px) in this environment regardless of the requested value, so the page laid out at 500px and got cropped into a 390px canvas rather than actually reflowing. Fixed by driving Chrome via the DevTools Protocol directly (`Emulation.setDeviceMetricsOverride`, width 390 / height 844 / mobile:true), which forces a true mobile layout viewport independent of the OS window size. Re-captured, verified no clipping (confirmed against a live `window.innerWidth` readout of 390 baked into a debug render before the final captures).

- `hero.webp` — hero + stat row + dual CTA, nav fully visible including "Book now"
- `venue-finder.webp` — search bar, state chips, populated venue cards (confirms the finder-as-hero-feature promise from `STRATEGY.md` Phase 4 works at mobile width)
- `parties.webp` — audience-split cards (Parties / Schools & Camps / Corporate)

## Not covered this run — handoff

- **Exact YouTube clip selection + re-edit/reframe/grade/upscale pass** — needs MotionDesigner + real channel access (YouTube Data API or manual scrub); this run only confirms the source exists and defines the treatment.
- **Scroll Scene 03 (Freestyle Academy)** — motion asset, MotionDesigner scope per `STRATEGY.md` Production Plan.
- **26 venue-detail hero galleries** — out of scope for the homepage reference build; template defined in `STRATEGY.md` Phase 4, not yet produced.
