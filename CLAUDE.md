# quarto-ext

Quarto extensions for the r-dcm / measr brand: `rdcm-slides` and
`measr-slides` (revealjs), `measr-report` (pdf). Each directory has a
`template.qmd` that exercises every layout; render it to check changes.

## rdcm-slides: PDF export must keep working

Decks are exported to PDF through reveal's `?print-pdf` mode (open the
rendered HTML with `?print-pdf`, print to PDF from Chrome) and uploaded to
conference portals. Every layout change to `theme.scss`, `hexagons.css`, or
`assets.css` has to hold up there as well as on screen.

What print mode does to the slides:

- Adds `html.reveal-print` and applies
  `html.reveal-print .reveal .slides section { padding: 0 !important; display: block !important; position: absolute !important; ... }`.
- Measures each slide's `scrollHeight` *before* wrapping it in `.pdf-page`,
  then writes an inline `top` to centre `.center` slides. A layout built on
  `height: 100%` + flex centring measures as near-zero at that moment and is
  pushed to the bottom of the page.

Rules that follow from that (all in `rdcm-slides/_extensions/rdcm-slides/theme.scss`):

- Slide geometry is expressed only through `--rdcm-inset-top/right/bottom/left`.
  The `padding` shorthand is written exactly twice: once for screen near the
  top of the file, once with `!important` in the `html.reveal-print` block at
  the end. A new layout sets the four variables — never a bare `padding`, or
  the print block cannot restore it.
- A selector that has to beat the print reset needs more specificity than
  `html.reveal-print .reveal .slides section` = (0,3,2).
  `.reveal .slides section.closing` (0,3,1) loses; `.reveal .slides section.slide.dark` (0,4,1) wins.
- Any new flex-centred / full-height layout goes in the print block's
  `display: flex; top: 0; height: 100%` rule alongside `#title-slide`,
  `.dark`, `.hex-loud`, `.closing`.

Verify before calling a layout change done (the in-app browser cannot reach
localhost, so use headless Chrome):

```bash
python3 -m http.server 8765 -d rdcm-slides &
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --disable-gpu \
  --window-size=2112,1188 --no-pdf-header-footer --run-all-compositor-stages-before-draw \
  --virtual-time-budget=40000 --print-to-pdf=out.pdf \
  "http://localhost:8765/template.html?print-pdf"
```

`template.qmd` should give 29 pages; a 1-page PDF means the print hook raced
the load — rerun with a larger `--virtual-time-budget`. Rasterise with
`pdftoppm -png -r 24` and compare against `--screenshot` captures of the same
slides (`template.html#/<slide-id>`). Iconify icons can be missing in headless
prints because they are fetched over the network at load; that is not a theme
bug. Hex clusters sized in `vmax` are fine in print — switching to `cqmax` was
tried and made no difference.
