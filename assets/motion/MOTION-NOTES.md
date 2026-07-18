# Motion Assets — Phase 5 Scene Map

Direction A "Suspended". All clips generated via Higgsfield (`cinematic_studio_video_v2`, genre `action`/`auto`, `mode: pro`, sound off) or re-graded from real BOUNCE Australia YouTube footage. Colour grade approximates the brand LUT (crushed blacks, volt/magenta lift) via `ffmpeg eq` + `colorbalance` — swap for a true `.cube` LUT when the colourist delivers one.

| File | Phase 5 scene | Slot in `index.html` | Notes |
|---|---|---|---|
| `hero-loop.mp4` + `hero-loop-poster.jpg` | Hero float (ambient loop, `index.html:263-266` `.hero-card`) | Hero visual card | 7s, 3:4, slow ambient drift. `<video autoplay muted loop playsinline preload="none" poster="hero-loop-poster.jpg">`; swap for `<img>`/poster only when `prefers-reduced-motion: reduce` (STRATEGY §Phase 5 "Static composition"). |
| `scene-01-venue-reveal.mp4` + `-poster.jpg` | Inside-Bounce story row 1 (`index.html:334`) | Scroll scene 01 | 6s, 16:9, slow parallax push through connected courts. Plays inline on scroll-into-view (`IntersectionObserver`), no pin. Stacked/fade-only on mobile per Phase 5 table. |
| `scene-02-airbag.mp4` + `-poster.jpg` | Inside-Bounce story row 2 (`index.html:342`) — the pinned scroll-scrubbed sequence | Scroll scene 02 | 6s, 16:9, slow-mo flip into airbag. Encoded with dense keyframes for reliable `currentTime` seeking during scroll scrub. **Desktop only**: pin `.story-row` and scrub playback position to scroll offset (`video.currentTime = progress * video.duration`, markup starts `preload="none"`, then JS fetches the 2.6 MB clip near the scene and attaches it as a blob URL so Pages preview playback is seekable, no `autoplay`). **Mobile**: render as a normal inline `<video controls muted playsinline poster="...">`, autoplay off, no pin. **Reduced motion**: swap to `scene-02-airbag-poster.jpg` only, no `<video>` element mounted. |
| `scene-03-academy.mp4` + `-poster.jpg` | Inside-Bounce story row 3 (`index.html:350`) | Scroll scene 03 | 6s, 16:9, coach + squad character motion. Same inline/parallax treatment as scene 01. |
| `reuse-this-is-bounce-16x9.mp4` / `reuse-this-is-bounce-4x5.mp4` | Reuse pass — real footage | Gallery / social proof / marquee background (not yet slotted) | Source: BOUNCE Australia YouTube, "This is BOUNCE" (`youtube.com/watch?v=tLJ-XSJEGrY`), re-graded to brand LUT, reframed 16:9 (native) + 4:5 (centre crop), H.264, `faststart`, silent. Centre-crop reframe chosen over Higgsfield's content-aware reframe for cost/time — re-run through `reframe` (topaz/pro) if the 4:5 crop clips action on a specific shot. |

## Implementation notes (for the Phase 7 build / whoever wires these into `index.html`)
- All clips are silent (`sound:"off"`) — no autoplay-with-sound issue.
- All `<video>` tags should start with `preload="none"` (STRATEGY §Phase 5 perf column) so nothing downloads until in view; pair with the matching `poster` JPG so there's no layout flash. Scene 02 is the exception after lazy trigger: JS fetches the clip into a blob URL near the pinned scene because scroll-scrubbing requires seekable video data on the Pages preview.
- `prefers-reduced-motion: reduce` → don't mount any `<video>`; render the poster JPG as a static `<img>` instead (already the pattern used for `.hero-card .figure` fallback).
- Airbag scene is the only pinned/3D-adjacent sequence per STRATEGY — Three.js scroll-scrub reads `scene-02-airbag.mp4` frame-accurately via `requestVideoFrameCallback`/`currentTime`, not the raw DOM scroll handler used for the other two rows.
- File sizes kept under ~2.5MB (generated clips) / ~7.5MB (31s reuse clip) at 1280px wide, H.264 `faststart`, to stay lazy-load friendly on mobile data.

## Open follow-up
- Only one reuse clip processed ("This is BOUNCE" — the best brand montage on the channel). Channel has ~26 more videos (Friday Super Sessions, miniBOUNCE, Ultimate Parties, etc.) that are candidates for the venue/parties/membership sections outside this issue's scope (hero + 3 Inside-Bounce scenes) — flagging for StudioDesigner/asset-inventory pass (WEB-211) rather than duplicating here.
- Colour grade is an ffmpeg approximation, not a delivered `.cube` LUT — fine for the reference build, worth formalising if this goes to a real colourist for the Next.js production build.
