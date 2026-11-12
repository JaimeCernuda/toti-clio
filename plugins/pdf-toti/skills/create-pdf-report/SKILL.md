---
name: create-pdf-report
title: Create a Profit Connect PDF Report
description: Create a polished, Profit Connect-branded PDF report when the scientist explicitly requests a PDF deliverable. Ordinary reports remain Markdown unless another format is requested.
keywords:
- level:L0
- report
- pdf
---

# Create a PDF Report

Load this skill only when the scientist asks for a PDF report or another PDF
deliverable. A request for a report without a requested file format should
produce Markdown, not a PDF.

## Build from grounded content

Establish the report's claims, data, citations, and intended audience before
laying out pages. Preserve units, uncertainty, provenance, and distinctions
between supplied facts, observed results, and inference. Do not add decorative
specificity that the evidence does not support.

Generate the PDF directly with ReportLab. Prefer flowable layout for reports
whose text or page count may change; use fixed coordinates only for short,
deliberately composed layouts. Keep the builder and temporary assets outside
the final output directory, and preserve source assets.

If the report benefits from illustrations, use supplied figures first. Create
new images only when the scientist requests them or they materially improve
the deliverable, and label generated images honestly.

## Apply the Profit Connect brand

Every PDF report from this skill carries the Profit Connect identity
(profitconnect.co.uk, revenue recovery audits) unless the scientist asks for an
unbranded document.

Resolve this skill's directory as `SKILL_ROOT`. The logo is
`SKILL_ROOT/assets/profit-connect-logo.png`: the orange "p" mark plus the
"profit connect" wordmark, 600 x 167 px (about 3.6 : 1), transparent background.
Never redraw, recolor, stretch, or crop it; scale it uniformly and place it only
on white or very light backgrounds.

Slogan: **"Detection and recovery, our solution for your business"**. Use this
exact wording; do not paraphrase it or invent other taglines.

Palette, from the company website's theme:

| Role | Hex | Use |
| --- | --- | --- |
| Profit orange | `#EF7D00` | Accent rules, callout bars, key-figure highlights, chart primary series |
| Navy | `#14133B` | Headings, table header fills, cover title |
| Charcoal | `#3C3C3B` | Body text, running header/footer text, table borders |
| Light orange | `#FF9933` | Secondary accent, chart secondary series |
| Light gray | `#F2F5F7` | Alternating table rows, callout backgrounds |
| Rule gray | `#EDEDED` | Hairlines and subtle dividers |
| Paper | `#FFFFFF` | Page background |

Contrast rules: orange on white is about 2.8 : 1, so never set text in orange
(or light orange) on a light background, and never set white text on orange.
Use orange for rules, bars, fills, and chart marks. Text on an orange fill is
Navy. Navy and Charcoal on white or Light gray are fine at any size.

Layout:

- **Cover / first page:** logo at the top left, about 2.2 in (160 pt) wide. The
  logo already contains the "profit connect" wordmark, so do not repeat the
  name next to it. Under it, the slogan in 10-11 pt italic Charcoal. Then the
  report title in Navy, subtitle and date in Charcoal, and a 3 pt Profit orange
  rule under the title block.
- **Running header (pages after the first):** logo about 0.3 in (22 pt) tall at
  the left, the report title in 8-9 pt Charcoal at the right, and a 1 pt Profit
  orange rule underneath.
- **Footer (every page):** the slogan at the left and "Page N of M" at the
  right, in 8 pt Charcoal, above a 0.5 pt Rule gray line.
- **Typography:** the website uses Archivo for headings and Barlow for body
  text. Use static (non-variable) TTFs of those families when the prepared
  runtime provides them or the scientist supplies them, registered with
  ReportLab's `TTFont`. Otherwise fall back to Helvetica-Bold for headings and
  Helvetica for body text; do not download fonts without asking. H1 in Navy
  with a short Profit orange underline; H2 and H3 in Navy.
- **Tables:** Navy header row with bold white text, Light gray banding,
  Charcoal 0.5 pt grid. Highlight a key row or cell with a Profit orange left
  bar, not orange text.
- **Charts:** Profit orange for the primary series, Navy for the secondary,
  then Light orange and grays; keep data colors distinct where a chart has more
  series.

Implement the header and footer with ReportLab `onFirstPage` / `onLaterPages`
callbacks (or `PageTemplate` frames) so they repeat on every page, and use a
two-pass build or a `NumberedCanvas` to print the total page count. Draw the
PNG logo with `mask="auto"` so its transparency is kept.

## Verify the artifact

After each meaningful layout change:

1. Reopen the PDF with `pypdf` and verify page count, expected content, and
   document metadata.
2. Render every page to PNG with Poppler.
3. Inspect every rendered page with `view_image` for clipping, overlap,
   unreadable text, poor spacing, broken glyphs, misplaced figures, and weak
   page transitions. Also confirm the branding: the logo is sharp and
   undistorted, the header and footer appear on every page with correct page
   numbers, the slogan is spelled exactly, the brand colors are applied as
   specified, and no text is set in orange on a light background.
4. Correct defects and repeat the checks. Content extraction alone is not
   evidence of visual correctness.

Use `uv` for Python execution and dependency management. Typical dependencies
are `reportlab`, `pypdf`, and Pillow; use only the packages the report needs.
Place final deliverables under `output/pdf/` when the workspace has no more
specific output convention, and keep intermediate renders under `tmp/pdfs/`.

Deliver the final PDF only after both logical and visual verification pass.
