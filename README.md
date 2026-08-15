# A5 Notebook Template Generator

A LaTeX template for generating printable A5 notebook pages with customizable styles.
Creates PDF output formatted for double-sided (duplex) printing on A4 paper (two
A5 pages per side, four per sheet), using standard saddle-stitch booklet
imposition, for folding and hand-sewing into a booklet.

## Features

- Three page styles:
  - Ruled lines
  - Grid pattern
  - Dot grid
- Configurable parameters:
  - Line/grid/dot spacing
  - Margins
  - Shading intensity
  - Dot size
  - Page numbers
  - Number of sheets
- Optimized for black & white printing
- Proper A5 sizing for duplex printing on A4, with correct booklet imposition
- Binding gutter and sewing-guide marks

## Requirements

- LaTeX distribution (TeXLive, MiKTeX, etc.)
- Required packages:
  - `geometry`
  - `tikz`

## Files

- `a5notebook_common.tex` — shared engine: all configurable parameters,
  page-drawing macros, and the notebook generators (not compiled directly):
  - `\generatenotebookduplex{TYPE}` — double-sided, with booklet imposition.
    Used by the three full-notebook files below.
  - `\generatenotebook{TYPE}` — single-sided, sequential page numbers.
    Used only by the `notebook.tex` sampler; not imposed, so don't use it
    to print an actual bindable notebook.
- `a5notebook_ruled.tex`, `a5notebook_grid.tex`, `a5notebook_dotgrid.tex` —
  compile one of these to get a full duplex notebook of that single style
  (`\numsheets` A4 sheets, default 20 → 80 A5 pages).
- `notebook.tex` — a 3-page sampler (one sheet of each style), for
  previewing settings before committing to a full print run.

## Usage

1. For a full single-style notebook, compile one of:
```bash
lualatex a5notebook_ruled.tex
lualatex a5notebook_grid.tex
lualatex a5notebook_dotgrid.tex
```
   Or preview all three styles at once with `lualatex notebook.tex`.

2. Print the resulting PDF:
   - Use A4 paper
   - Select landscape orientation
   - **Double-sided (duplex)**, scale set to none/100%/actual size (not
     "fit to page"), normal or higher print quality (avoid toner-saver/
     draft/eco modes — see "Print crispness" below)
   - See "Duplex binding-edge setting" below before committing to the
     full print run

3. Assemble: stack the printed sheets in page order (sheet 1 on top —
   check which end of your printer's output stack that is, since it
   varies by printer), sew through the center line (see "Sewing marks"
   below), then fold the whole stack in half along the spine.

## Customization

Edit these variables at the top of `a5notebook_common.tex` (all three
full-notebook files and the sampler share this one file):

```latex
\def\linespacing{7}     % Spacing between ruled lines (mm)
\def\gridspacing{7}     % Square grid size (mm)
\def\dotspacing{5}      % Spacing between dots (mm)
\def\dotsize{0.15}      % Dot radius (mm)
\def\shadeval{100}      % Ink darkness percentage (0-100)
\def\strokewidth{0.15}  % Line width for ruled/grid/dot marks (mm)
\def\pagemargin{10}     % Outer side margin (mm)
\def\gutter{6}          % Extra margin on the spine side, for binding (mm)
\def\vtopmargin{10}     % Top margin (mm)
\def\vbottommargin{15}  % Bottom margin (mm) - leaves room for the page number
\def\sewingmargin{12}   % Sewing marks stay this far from the top/bottom (mm)
\def\startpage{1}       % First page number
\def\numsheets{20}      % A4 sheets to generate (each sheet = 4 A5 pages,
                        % duplex) - 20 sheets = 80 A5 pages
```

The square grid always lands flush at the top-left corner formed by
`\pagemargin`/`\vtopmargin` (plus `\gutter` on whichever side faces the
spine). The dot grid's top-left dot sits exactly `\dotspacing` from the
top and left borders (also plus `\gutter` on the spine side); its bottom
uses `\vbottommargin` instead, so dots don't run into the page number.

### Binding gutter

Each A5 spread is printed side by side on one A4 sheet; the seam between
the two pages is the spine. `\gutter` adds extra blank margin on the
spine side of every page (in addition to `\pagemargin`) to leave room for
folding and sewing — e.g. stacking A4 sheets, sewing along the center
line, then folding into a saddle-stitched booklet. Increase it for
thicker binding (more sheets) or a wider seam allowance.

### Sewing marks

Five tiny points are drawn on the spine as awl-punch guides for a 5-hole
pamphlet stitch: one at the vertical center of the page, one pair at
`\sewingmargin` from the top/bottom edges, and one pair at the midpoints
between those and the center. Push the awl through each point to punch
the holes before stitching. In `\generatenotebookduplex` the points are
only drawn on the back (inside) of each sheet — the front is the
outward-facing cover/spread you don't see while sewing through the fold
from inside, so marking it would just be clutter.

### Duplex booklet imposition

`\generatenotebookduplex` uses standard saddle-stitch imposition: for an
`\ntotal`-page notebook, sheet `k` (1 = outermost) prints
`[\ntotal-2k+1 | 2k-2]` (offset by `\startpage`) on the front and
`[2k-1 | \ntotal-2k]` on the back. PDF pages come out in sheet order
(sheet 1 front, sheet 1 back, sheet 2 front, ...), which is what a duplex
printer expects: odd PDF pages become sheet fronts, even PDF pages become
the backs of those same sheets, in order.

### Duplex binding-edge setting

Whether your printer's "long-edge" or "short-edge" duplex setting
produces a correctly-aligned (not mirrored or upside-down) back side for
this landscape layout depends on your printer/driver and can't be
predicted from the LaTeX source. **Before running the full job**, set
`\numsheets{2}` and print just that 2-sheet (8-page) test: sheet 1's
back should show pages 2 and 7, right-side up, aligned with the front's
spine. If it's flipped or mirrored, switch the duplex edge setting and
reprint the test — cheaper than discovering it on sheet 15 of 20.

### Print crispness on a monochrome laser printer

A monochrome laser has no true gray — any partial-opacity tint gets
halftoned, and thin lines or small dots are especially prone to printing
patchy or broken rather than as a clean stroke. Rather than a light gray
wash, this template defaults to solid black (`\shadeval{100}`) at a thin,
configurable stroke width (`\strokewidth`) — lightness comes from the
line/dot being small, not from ink opacity. This is tool-independent
(the printer's halftone engine treats any PDF's gray fills the same way
regardless of what generated them); it's a print-target design choice,
not a LaTeX limitation. Test-print a page and tune `\strokewidth`,
`\dotsize`, and `\shadeval` to taste.

## Output

- `a5notebook_ruled.pdf` / `a5notebook_grid.pdf` / `a5notebook_dotgrid.pdf`:
  `2 * \numsheets` PDF pages (default 40, for `\numsheets{20}`) — one
  front + one back per sheet — imposed for duplex printing onto
  `\numsheets` physical A4 sheets, yielding `4 * \numsheets` A5 notebook
  pages (80 by default).
- `notebook.pdf`: 3 A4 pages, one sheet of each style (pages 1-6), for
  a quick preview (single-sided, not imposed — for visual preview only).
- Page numbers in outer bottom corners
- Solid, thin-stroke patterns for crisp output on monochrome laser printers
- Binding gutter and 5-hole pamphlet-stitch punch guides on the spine

## Before use

- Ensure printer margins are set to minimum/none to avoid scaling
- Run the 2-sheet duplex test described above to confirm the binding-edge
  setting before the full print run

## License

GNU General Public License v2.0 (GPL-2.0)

This program is free software; you can redistribute it and/or modify it under
the terms of the GNU General Public License as published by the Free Software
Foundation; either version 2 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT
ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS
FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along
with this program; if not, write to the Free Software Foundation,
Inc., 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301, USA.
