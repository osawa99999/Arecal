# Handover Prompt — Copyright / Legal Risk Check for VoldiaM

## Context
A copyright/legal-risk audit was already performed on the sibling app **AreCal / Arecalay** (2026-07-22, this session). This prompt summarizes that methodology and its findings so the same checklist can be applied to **VoldiaM** (single-file HTML excavation-volume tool, orange theme, main file `VoldiaMv00xx.html` + `voldia3d.js`).

The client (a construction-site manager, not a programmer) explicitly wants a thorough ("しつこいくらい") check. Do not soften findings to make him feel better — flag anything genuinely uncertain and say so plainly.

## Checklist used for AreCal/Arecalay (apply the same steps to VoldiaM)

1. **Scan for embedded third-party license text.**
   `grep -n -iE "copyright|license|©|all rights reserved|proprietary"` across every `.html`/`.js` file. Any hit must be verified: is the copyright/license notice still intact and unmodified (required by permissive licenses like MIT/BSD/Boost)? Never strip or rewrite these.

2. **Enumerate all external resource loads.**
   `grep -n -iE "<script[^>]*src=|<link[^>]*href=|https?://"` — check every CDN script (e.g. pdf.js, xlsx.js, jsPDF, and for VoldiaM likely `manifold-3d`/wasm or `three.js`) against its actual license (OSS license loaded via reference only = fine; embedding modified/relicensed copies is not).

3. **Check embedded/inline third-party libraries.**
   If any library is bundled inline (not loaded via CDN) — e.g. AreCal's inline ClipperLib (Boost License) + jsbn (BSD License) — confirm the original copyright header is preserved verbatim immediately before/around the library code. This was compliant in AreCal/Arecalay.

4. **Check fonts.**
   `grep -iE "font-family|@font-face|googleapis|fonts\.|typekit"` — system font references only (e.g. `'Segoe UI'`) are not a copyright concern; embedded/downloaded proprietary font files would be.

5. **Check images/logos.**
   Any embedded base64 image or logo — confirm origin. In AreCal/Arecalay, the helmet logo was confirmed by the client as made in-house (by a family member), so no issue. For VoldiaM, ask the same question if any logo/image asset exists.

6. **Check SVG/icon/CAD-derived visual assets — the highest-risk category found so far.**
   For AreCal/Arecalay's machinery icons (used identically in VoldiaM's UI if shared), the client confirmed:
   - CAD data is ~60% traced from manufacturer drawings, but decorative elements (paint lines, logos) and fine structural/dimension lines are NOT traced.
   - Shapes are heavily simplified (manufacturer drawings are far too detailed; simplification is deliberate) to the point where only the generic machine type (e.g. "this is a backhoe") is recognizable, not the source manufacturer.
   - Multiple manufacturers' features are blended, so no single manufacturer's design can be identified from the asset.
   - Highly generic shapes (e.g. scaffolding/足場) are not traced from anything specific — general/functional shapes have low copyright protection to begin with (functional/utilitarian form is not what copyright protects; copyright protects expression, e.g. decorative/paint elements — which were excluded here).
   - Rare/distinctive machinery is deliberately excluded from the icon set.
   - Some base assets started from free stock CAD blocks, but were rescaled non-uniformly (different X/Y scale factors) before retracing — i.e., not used as-is.
   - **Conclusion reached for AreCal/Arecalay**: risk is low given the above, PARTICULARLY because the app is currently internal-use-only (社内利用限定, per the disclaimer header). If/when the app is ever sold externally or made public, this should be reassessed more rigorously — recommend consulting an actual lawyer at that point. This is not a substitute for legal advice; I am not a lawyer.
   - **For VoldiaM: ask the client whether it uses the same asset pipeline/CalayMachineryData.dat as AreCal/Arecalay, or a separate/different set of icons or CAD-derived visuals.** If separate, repeat the same origin questions (traced from manufacturer drawings? how much simplified/blended? free CAD blocks used as-is or modified?).

7. **Check for near-duplicate UI elements causing confusion (not a copyright issue, but flag it).**
   Example already fixed in AreCal/Arecalay: the "線" (line) and "距離測定" (distance measurement) buttons both used the same 📏 emoji, which was confusing. Changed the distance-measurement icon to 📐 across both button labels and all related status-bar text (in both `AreCal_v0_04xx.html` and `placement_v2.js`). Check VoldiaM's UI for any similar icon collisions.

## Findings already confirmed for AreCal/Arecalay (for reference, not to be re-verified unless something changed)
- CDN libraries (pdf.js, xlsx.js, jsPDF): loaded by reference only, all OSS-licensed — no issue.
- Inline ClipperLib (Boost License) + jsbn (BSD License): license headers intact — no issue.
- Fonts: system font only, no embedding — no issue.
- Logo: confirmed in-house/family-made — no issue.
- Machinery SVG icons: see point 6 above — low risk given current internal-use-only status; revisit if externalized.
- Company name in disclaimer text: standardized to "大崎建設株式会社 ICT推進室" (removed "（旧称・愛称：OSAKI Tech Lab）" — the nickname was deemed unnecessary in the formal disclaimer).

## What to actually do for VoldiaM
1. Run steps 1–5 above against `VoldiaMv00xx.html` and `voldia3d.js` (and any other companion files).
2. Ask the client directly whether VoldiaM shares the same machinery-icon asset pipeline as AreCal/Arecalay, or has its own. Apply step 6's questions accordingly.
3. Report findings the same way: a clear per-item table (item checked → result), plus explicit flags for anything that isn't clean, without hedging.
4. Any code fix made as part of this check still follows the normal versioning rules: bump `VoldiaMv00xx.html`'s filename + internal version display + changelog comment, same as AreCal.
