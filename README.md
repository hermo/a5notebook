# A5 Notebook Template Generator

A LaTeX template for generating printable A5 notebook pages with customizable styles.
Creates PDF output formatted for duplex printing on A4 paper (two A5 pages per sheet).

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
- Optimized for black & white printing
- Proper A5 sizing for duplex printing on A4

## Requirements

- LaTeX distribution (TeXLive, MiKTeX, etc.)
- Required packages:
  - `geometry`
  - `tikz`

## Usage

1. Compile with:
```bash
lualatex notebook.tex
```

2. Print the resulting PDF:
   - Use A4 paper
   - Select landscape orientation
   - Enable duplex printing for double-sided notebooks

## Customization

Edit these variables at the top of `notebook.tex`:

```latex
\def\linespacing{7}    % Spacing between lines (mm)
\def\gridspacing{7}    % Grid size (mm)
\def\dotspacing{7}     % Spacing between dots (mm)
\def\dotsize{0.5}      % Size of dots (mm)
\def\shadeval{20}      % Opacity percentage (0-100)
\def\pagemargin{10}    % Page margin (mm)
\def\startpage{1}      % First page number
```

## Output

- 3 A4 pages containing:
  - Pages 1-2: Ruled lines
  - Pages 3-4: Grid pattern
  - Pages 5-6: Dot grid
- Page numbers in outer bottom corners
- Light gray patterns optimized for B&W printing

## Before use

- Ensure printer margins are set to minimum/none to avoid scaling
- Test print one page first to verify alignment

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
