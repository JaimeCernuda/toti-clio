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

Every PDF report from this skill carries the Profit Connect identity unless the
scientist asks for an unbranded document.

Resolve this skill's directory as `SKILL_ROOT`. The logo is
`SKILL_ROOT/assets/profit-connect-logo.png` (167 x 167 px, square, white
background). Never redraw, recolor, stretch, or crop it; scale it uniformly.

Brand name: **Profit Connect**. No official slogan has been supplied, so do not
invent one or print a tagline. If a slogan is added later, put it on the cover
under the brand name and in the running header.

Palette, taken from the logo:

| Role | Hex | Use |
| --- | --- | --- |
| Profit green | `#A0C651` | Accent rules, table header fills, callout bars, chart highlight series |
| Charcoal | `#535150` | Headings, running header/footer text, table borders, secondary chart series |
| Ink | `#000000` | Body text |
| Paper | `#FFFFFF` | Page background |
| Green tint | `#ECF4DC` | Alternating table rows, callout backgrounds |

Contrast rules: green on white is too low-contrast for text, so never set text
in green on a light background. Text placed on a green fill is Ink or Charcoal.
Charcoal on white is fine for headings and small text.

Layout:

- **Cover / first page:** logo at the top left, about 1.1 in (80 pt) wide.
  The logo already contains the "Profit Connect" wordmark, so do not repeat the
  name next to it. Then the report title, subtitle and date, and a 3 pt Profit
  green rule under the title block.
- **Running header (pages after the first):** logo about 0.35 in (25 pt) wide
  at the left, the report title in 8-9 pt Charcoal at the right, and a 1 pt
  Profit green rule underneath.
- **Footer (every page):** "Profit Connect" at the left and "Page N of M" at
  the right, in 8 pt Charcoal, above a 0.5 pt Charcoal rule.
- **Typography:** Helvetica-Bold for headings and Helvetica for body text,
  which matches the logo's bold sans-serif wordmark. H1 in Charcoal with a
  short Profit green underline; H2 and H3 in Charcoal.
- **Tables:** Profit green header row with bold Ink text, Green tint banding,
  Charcoal 0.5 pt grid.
- **Charts:** Profit green for the primary series, Charcoal for the secondary
  one, then lighter grays; keep the data colors distinct where a chart has more
  series.

Implement the header and footer with ReportLab `onFirstPage` / `onLaterPages`
callbacks (or `PageTemplate` frames) so they repeat on every page, and use a
two-pass build or a `NumberedCanvas` to print the total page count.

## Verify the artifact

After each meaningful layout change:

1. Reopen the PDF with `pypdf` and verify page count, expected content, and
   document metadata.
2. Render every page to PNG with Poppler.
3. Inspect every rendered page with `view_image` for clipping, overlap,
   unreadable text, poor spacing, broken glyphs, misplaced figures, and weak
   page transitions. Also confirm the branding: the logo is sharp and
   undistorted, the header and footer appear on every page with correct page
   numbers, the brand colors are applied as specified, and no text is set in
   green on a light background.
4. Correct defects and repeat the checks. Content extraction alone is not
   evidence of visual correctness.

Use `uv` for Python execution and dependency management. Typical dependencies
are `reportlab`, `pypdf`, and Pillow; use only the packages the report needs.
Place final deliverables under `output/pdf/` when the workspace has no more
specific output convention, and keep intermediate renders under `tmp/pdfs/`.

Deliver the final PDF only after both logical and visual verification pass.
