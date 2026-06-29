# Homepage Update Spec — for Claude Code

## Context

This is a personal academic homepage built on the **Jon Barron single-file template**
(a single `index.html` with inline CSS, table/row-based layout, hosted on GitHub Pages).
The repo root contains `index.html` and an `images/` folder.

**Goal:** apply 5 design enhancements to the existing `index.html` *in place*. Keep the
overall Jon Barron look (centered ~820px column, clean sans-serif, blue/orange links).
Do **not** migrate to a framework or rewrite from scratch — edit the existing file.

A fully worked reference implementation is provided as `index_sample.html` (the version this
spec is derived from). Use it as the source of truth for exact markup/CSS, and adapt the
selectors to the existing file's structure where they differ.

---

## The 5 changes

### 1. Thin gray divider line between papers
Each publication entry is separated by a thin gray rule.

- Add `border-top: 1px solid #ececec;` to each paper row container (`.paper`).
- Remove the top border on the first entry: `.paper:first-of-type { border-top: none; }`.

### 2. Merge "Research" + "Journal" into one `Publications` section
The current page has two separate sections (conference "Research" and "Journal").
Combine them into a **single `Publications` section**, sorted **reverse-chronologically**
(2026 → 2024), interleaving conference and journal papers by year. Keep the venue label on
each entry so the type is still clear. Use the merged ordering in the Data section below.

### 3. Clear per-section dividers
Every section header (`News`, `Publications`, `Patents`) gets a strong bottom rule so
sections read as distinct blocks.

- `h2.section { border-bottom: 2px solid #1a1a1a; padding-bottom: 8px; margin: 48px 0 20px; }`

### 4. PDF / Code / Project as clickable buttons
Replace the plain text links (e.g. `[pdf] / [code]`) with pill-shaped buttons.

- Wrap links in `<div class="btn-row">` and give each `<a>` the class `btn`.
- Button CSS:
  ```css
  .btn-row { display: flex; flex-wrap: wrap; gap: 8px; }
  .btn {
    display: inline-block; padding: 3px 14px; font-size: 0.85rem;
    border: 1px solid #dcdcdc; border-radius: 999px;
    background: #f3f3f3; color: #1a1a1a;
    transition: background .15s ease, border-color .15s ease;
  }
  .btn:hover { background: #e7e7e7; border-color: #c8c8c8; color: #1a1a1a; }
  ```
- Button labels: `PDF`, `Code`, `Project Page`. Omit any button whose link doesn't exist.

### 5. TL;DR one-line summary per paper
Add a one-line summary under the venue line for each paper.

- Markup: `<p class="tldr">…</p>`
- CSS:
  ```css
  .tldr { margin: 0 0 10px; font-size: 0.92rem; color: #6b6b6b; }
  .tldr::before { content: "TL;DR  "; font-weight: 700; color: #999; }
  ```
- **Important:** the TL;DR texts in the Data section are *drafts derived from titles only*.
  Leave them in but keep the `(draft — replace…)` marker so the author knows to rewrite them.
  Do not invent results, metrics, or claims.

---

## Target HTML structure for one paper entry

Each publication should follow this pattern:

```html
<div class="paper">
  <div class="paper-thumb"><img src="images/KEY.png" alt="… teaser"></div>
  <div class="paper-body">
    <p class="paper-title">TITLE</p>
    <p class="paper-authors">AUTHORS (wrap "Jisoo Park" in <span class="me">…</span>)</p>
    <p class="paper-venue">VENUE (wrap acronym in <span class="highlight">…</span>)</p>
    <p class="tldr">TLDR <em>(draft — replace with your own one-liner)</em></p>
    <div class="btn-row">
      <a class="btn" href="…" target="_blank">PDF</a>
      <a class="btn" href="…" target="_blank">Code</a>
    </div>
  </div>
</div>
```

Supporting CSS for the row layout (add if not already present):

```css
.paper { display: flex; gap: 24px; padding: 22px 0; border-top: 1px solid #ececec; align-items: flex-start; }
.paper:first-of-type { border-top: none; }
.paper-thumb { flex: 0 0 200px; width: 200px; }
.paper-thumb img { width: 100%; border-radius: 4px; display: block; background: #f0f0f0; aspect-ratio: 16/10; object-fit: cover; }
.paper-body { flex: 1 1 auto; }
.paper-title { font-weight: 700; font-size: 1.05rem; margin: 0 0 4px; line-height: 1.4; }
.paper-authors { margin: 0 0 2px; font-size: .95rem; }
.paper-authors .me { font-weight: 700; }
.paper-venue { margin: 0 0 8px; font-size: .92rem; color: #6b6b6b; font-style: italic; }
.paper-venue .highlight { color: #f09228; font-style: normal; font-weight: 700; }

@media (max-width: 600px) {
  .paper { flex-direction: column; gap: 12px; }
  .paper-thumb { flex-basis: auto; width: 100%; }
}
```

---

## Publications data (merged, reverse-chronological)

`*` = equal contribution, `†` = corresponding author. Add a note line under the section
header: `* denotes equal contribution.  † denotes corresponding author.`

| # | img key | Title | Authors | Venue | Links | TL;DR (draft) |
|---|---------|-------|---------|-------|-------|----------------|
| 1 | c3asd | C³ASD: Multi-Level Consistency-Driven Representation Learning for Robust Active Speaker Detection | Jin Hong\*, **Jisoo Park**\*, Junseok Kwon† | ECCV, Malmö, Sweden, 2026 | PDF=(none yet), Code=(none yet) | Multi-level consistency-driven representation learning for robust active speaker detection. |
| 2 | urhead | URHead: A Unified UV-Space Representation for Joint Mesh–3DGS Optimization in Head Avatars | Seonghak Lee, Junhee Cho, **Jisoo Park**, Min-Gyu Park, Jongmin Lee, Ju Hong Yoon, Junseok Kwon† | ECCV, Malmö, Sweden, 2026 | PDF=(none yet), Code=(none yet) | A unified UV-space representation that jointly optimizes mesh and 3D Gaussian Splatting for head avatars. |
| 3 | wildtalker_inf | WildTalker∞: Pushing the Limits of 3D Talking Portrait Synthesis in Unconstrained Environments | Seonghak Lee\*, **Jisoo Park**\*, Junseok Kwon† | T-ASLP, vol. 34, pp. 1259–1271, 2026 *(journal)* | PDF=https://ieeexplore.ieee.org/document/11394816 | Pushing 3D talking portrait synthesis toward unconstrained, in-the-wild environments. |
| 4 | univoicelite | Lightweight Wasserstein Audio-Visual Model for Unified Speech Enhancement and Separation | **Jisoo Park**\*, Seonghak Lee\*, Guisik Kim, Taewoo Kim, Junseok Kwon† | ASRU, Honolulu, Hawaii, USA, 2025 | PDF=https://arxiv.org/abs/2512.06689, Code=https://github.com/jisoo-o/UniVoiceLite | A lightweight audio-visual model that unifies speech enhancement and separation. |
| 5 | ddml | Deep Disentangled Metric Learning | Jinhee Park, **Jisoo Park**, Dakyeong Na, Junseok Kwon† | AAAI, Philadelphia, PA, USA, 2025 | PDF=https://ojs.aaai.org/index.php/AAAI/article/view/34184, Code=https://github.com/jinnnnnnnnn/DDML | A metric learning approach that disentangles representations for image similarity. |
| 6 | vtsurf | VT-Surf: Visual Tracking with Switching Dynamics under Time-Series Forecasting | Seonghak Lee, **Jisoo Park**, Radu Timofte, Junseok Kwon† | IET Electronic Letters, Vol. 61, 2025 *(journal)* | PDF=https://ietresearch.onlinelibrary.wiley.com/doi/10.1049/ell2.70495 | Visual tracking with switching dynamics under time-series forecasting. |
| 7 | wildtalker | WildTalker: Talking Portrait Synthesis in the Wild | Seonghak Lee\*, **Jisoo Park**\*, Junseok Kwon† | ECCV Workshop (3D Modeling, Reconstruction, and Generation in the Wild), Milano, Italy, 2024 | Project=https://lseonghak.github.io/website/wildtalker/, PDF=https://openreview.net/attachment?id=296BblZfXx&name=pdf, Code=https://github.com/Lseonghak/WildTalker | Talking portrait synthesis robust to unconstrained, in-the-wild conditions. |
| 8 | potf | POTF: Prompt-based Object-centric Tensorial Field | Seonghak Lee\*, **Jisoo Park**\*, Junseok Kwon† | ICTC Workshop — Oral, Jeju Island, Korea, 2024 | PDF=https://ieeexplore.ieee.org/document/10827426 | A prompt-based, object-centric tensorial field representation. |
| 9 | sota | SOTA: Sequential Optimal Transport Approximation for Visual Tracking in Wild Scenario | Seonghak Lee, **Jisoo Park**, Radu Timofte, Junseok Kwon† | IEEE Access, Vol. 12, no. 1, pp. 177028–177037, 2024 *(journal)* | PDF=https://ieeexplore.ieee.org/document/10766609 | Sequential optimal transport approximation for visual tracking in the wild. |

For entries 1 and 2, render the PDF/Code buttons with `href="#"` as placeholders (links TBD).

---

## Notes & constraints

- **Edit `index.html` in place.** Preserve the existing `<head>`, font setup, bio/header
  block, and footer. Only restructure the publications area and add the CSS above.
- **Image paths** use the `images/KEY.png` keys in the table. If a file is missing, the gray
  placeholder box keeps the layout intact — do not error out.
- **Keep author/venue text exactly as given**; only `Jisoo Park` is bolded via `.me`.
- **Do not** change the existing News block content; just ensure its section header uses the
  new `h2.section` divider style.
- **Patents** section stays as a simple list; apply the `h2.section` header style to it too.
- After editing, verify it renders correctly at narrow widths (the `.paper` row should stack
  vertically on mobile per the media query).
- Footer: keep the "Web page design credit to Jon Barron" attribution.

## Suggested commit
```
git add index.html
git commit -m "Redesign publications: unified section, paper dividers, link buttons, TL;DRs"
git push
```
