# Gallery Cover Motion — working spec  
## Global rules (from Craig)  
* 15 cover layouts; gallery name + date come from the gallery; studio name can be swapped for a logo (all layouts except 1–4).  
* Cover fills the viewport until the chevron is clicked or the page scrolls.  
* Every layout gets a subtle text/logo animation on page load. Image animation is chosen per layout.  
* Layouts 1–5: 30% black tint over the image for readability.  
* Layouts 7–15: photographer can change the background color; text must adjust automatically for contrast. (Cover 6 prototype also given these controls.)  
* Photographers can pick a text color. Date text = the text color at 50% (over the photo on 1–5; mixed with the background on 6–15). Studio name, title (and frame lines on L2/L3) use the full text color.  
* Studio name displays exactly as typed — no forced uppercase.  
* Logo size: fits inside a 180×180px box (aspect kept, never upscaled) on covers 7–11 and 13; 140×140px on covers 5, 6, 12, 14 and 15. Cover 5 also caps it at 80% of the header band height. Layouts no longer set a fixed logo height. Adjustments for the larger logo: cover 6 shrinks the blob when a logo shows so the date clears the chevron; cover 12 (phone) tucks photo C left so it clears the logo; covers 14 and 15 use a minimum header height instead of a fixed one.  
* Responsive title: wraps at its design size (44px) instead of scaling with the viewport; shrinks only as a fallback — 1px steps until the longest word fits and the title is ≤4 lines, never below 30px.  
* Prototype images: gallery cover images/ in the connected folder. Sample logo: gallery-cover-sample-logo.png. Blob mask: gallery cover images/blob.svg (756×604). Blob motion reference: gallery cover images/blob-animation.mov.  
## Shared motion tokens  
* ease-out cubic-bezier(.22,.61,.36,1), ease-expo cubic-bezier(.16,1,.3,1), ease-draw cubic-bezier(.65,0,.35,1)  
* Text choreography (same moves everywhere; start times vary): studio name letter-spacing closes .22em→.02em with a 6px drop (1100ms); logo fades in with scale .96→1 and blur 4px→0 (1000ms); title lines rise from a mask (1200ms, +110ms per line); date fade-up 8px (900ms); chevron fades in then nudges 6px twice.  
* Scroll: photo parallax inside its frame (0.35× on 1–5, 0.12× on 7–11, 13, 14); per-photo depth on 12; cover text fades out.  
* prefers-reduced-motion: everything becomes a 400ms opacity fade; parallax and blob morph off.  
* Build (session workspace): base.html + build.py (1–5), base-split.html + build2.py (7–11), build3.py (12), lib4.py + build4.py (13–15), build5.py (6).  
## Image entrances (reusable)  
* **Mat open** (covers 4, 9, 10, 11, 12, 14): photo box opens outward from its center — clip-path inset 9%→0 with opacity 0→1 in the first 25%, 1800ms ease-expo — while the photo inside settles scale 1.1→1 over 2800ms ease-out. Same on phones.  
* **Circle open** (cover 9 alt): same as mat open but with clip-path circle 41%→50%, so the photo stays round throughout.  
* **Seam wipe** (covers 7, 8): photo opens from the edge facing the text toward the outer edge — clip-path wipe 150–1550ms ease-expo, photo settle 1.12→1 (150–2550). On phones it runs bottom → top.  
* **Band wipe** (covers 5, 13): logo settles in the band first (250ms), then the photo wipes away from the band (5: down, 13: up) 350–1750ms with a settle.  
* **Blob morph** (cover 6): SVG clipPath from blob.svg, driven by script (path interpolation, works in all browsers). Intro 0–2400ms: a small (42%), rounder, turned blob swells into a slightly oversized form with swapped lobes (ease-out cubic, first 55%), then settles into the final shape (ease-in-out). Photo inside settles 1.15→1 over 2800ms. Ambient loop after the intro (on by default, toggle in controls): blends the final shape with three variants (anchors shifted ~6–9% of the blob size, handle lengths ±12–14%) on overlapping 5.3s / 6.2s / 7.9s sine cycles, easing in over 1.2s. Craig approved; ambient amplitude raised to match blob-animation.mov.  
    * Image bleed: the morph reaches about 33px past the blob's left edge and 25px past the top/bottom (measured over 30s in the 756×604 viewBox), so the photo is sized 896×716 at (−70, −56) — about 37px of spare image on every side. Any production blob must size the photo past the morph's maximum reach, or the photo edge shows.  
## Layout status (prototypes in prototypes/ in the connected folder)  
* **1** Full-bleed, studio top-center, title bottom-right. Image: settle 1.07→1 + fade from black (0–2800). Text: studio 500, title 800, date 1250, chevron 1700.  
* **2** Full-bleed + thin frame inset. Image: soft settle 1.045→1. Frame traces: top from center 200–900 → sides down 780–1580 → bottom halves close at chevron 1460–2060. Text: studio 450, title 1000, date 1400, chevron 1900.  
* **3** Title + date in a centered bordered box. Image: focus pull blur 14px→0 + scale 1.04→1 (0–2400). Box: top/bottom from center 600–1400, sides 1100–1700. Text: studio 500, title 1150, date 1550, chevron 1900.  
* **4** Photo on white mat. Image: mat open. Text: studio 700, title 950, date 1400, chevron 1800.  
* **5** White header band with logo/studio name (dark), photo below. Band wipe (down). Text: title 1000, date 1450, chevron 1850.  
* **6** Studio/logo top-center, photo in the blob (756×604 at 1440), title + date centered below. Blob morph. Text: studio/logo 600, title 1300, date 1700, chevron 2100.  
* **7–11 (split family)**: text: studio/logo 700, title 900, date 1350, chevron 1750. No tint. Phones stack photo over text.  
    * 7: photo left 55.6% full height; text left-aligned in right panel. Seam wipe.  
    * 8: photo strip left 34.7%; logo + title centered in right panel. Seam wipe.  
    * 9: square photo inset; text bottom-aligned to its right. Mat open.  
    * 9 alt (gallery-cover-9-circle.html): circular photo inset; text block vertically centered on the circle. Circle open. Phones: circle centered with 24px side margins.  
    * 10: portrait 2:3 photo inset; text bottom-aligned to its right. Mat open.  
    * 11: centered text column left (logo default), landscape photo right. Mat open.  
* **12** Collage of five photos around centered text. Each photo mat-opens and rises 28px, 1600ms, staggered A 0 → B 140 → E 280 → D 420 → C 600ms. Text: studio/logo 900, title 1100, date 1550, chevron 1950. Scroll depth per photo (eased 0.14 lerp): A −0.16, B −0.26, C −0.42, D −0.10, E −0.22.  
* **13** (build of 5, mirrored): photo top 74%, colored band below with logo/studio left and title + date right-aligned (phones: band stacks, left-aligned). Band wipe (up). Text: logo 250, title 1000, date 1450, chevron 1850. No tint.  
* **14** (build of 11): logo top-center, landscape photo (1024×510 at 1440) centered, title + date centered below. Mat open. Text: logo 700, title 900, date 1350, chevron 1750.  
* **15** Logo top-center, three photos (sides 225×340, center 364×510, 12px gaps), title + date below. Stack rises in (150–1050ms) with side photos tucked behind the center (offset 96% of width, ∓4°), then sides fan out (950–2150ms, ease-expo). Photo settle 1.1→1. Text: logo 300, title 1500, date 1900, chevron 2300.  
* **Background + auto contrast (6–15)**: background swatches (White, Linen, Stone, Sage, Slate blue, Espresso, Charcoal, Black) + custom picker. Text auto-picks #1D1F21 or white, whichever has the higher WCAG contrast; photographer can override. Chevron uses the 50% date color. Single-color logos invert on dark backgrounds. Prototype shows live contrast ratios.  
* Open issue: date at 50% is below 4.5:1 on most backgrounds (≈3.3:1 on white) — Craig to decide whether to keep 50% or raise it.  
* All 15 layouts have prototypes, plus the cover 9 circle alternate.  
