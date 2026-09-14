# r-dcm slides

A [Quarto](https://quarto.org) extension for Reveal.js presentations about the
[r-dcm](https://github.com/r-dcm) suite of R packages.

Slides are 1920×1080. Backgrounds are drawn with scalable hexagon vectors instead of PowerPoint
screenshots, so slides fill any display without letterboxing.

## Install

```bash
quarto use template r-dcm/quarto-ext/rdcm-slides
```

## Slide classes

| Class | Effect |
| --- | --- |
| *(default)* | White slide, rotating hexagon clusters in opposite corners |
| `.dark` | Navy slide with drifting light hexagons |
| `.closing` | Navy, centred — Q&A / thank-you |
| `.empty` | No hexagons at all (white); symmetric margins, so centred figures land on the slide's centre |
| `.empty-navy` | Same bare canvas on the navy field |
| `.hex-quiet` | Top-left cluster only, slightly shrunk; bottom-right suppressed — use behind wide figures |
| `.hex-loud` | Oversized clusters for statement slides |
| `.exercise` | Light blue field with navy/red clusters — signals "your turn" |
| `.thank-you` | Navy closing slide: centred title over a two-column body (visual left, contacts right) |
| `.no-rule` | Drops the red hex-tipped rule under a heading |

Level-1 headings become section dividers automatically.

## Callouts

Quarto's five callout types are restyled as soft tinted panels — no boxed
borders, no coloured title bars — each with a hexagon in place of the default
Bootstrap glyph. Tint and glyph are drawn from the same palette colour in
every case:

| Callout | Field | Glyph |
| --- | --- | --- |
| `.callout-note` | Light blue tint | Light blue hexagon |
| `.callout-tip` | Blue tint | Blue hexagon |
| `.callout-warning` | Red tint | Red hexagon |
| `.callout-caution` | Cream tint | Cream hexagon |
| `.callout-important` | Navy tint | Navy hexagon |

```markdown
::: {.callout-tip}
## Tip
Cached fits make re-rendering slides cheap.
:::
```

The title is optional; without a heading the callout renders as a bare tinted
panel with its glyph. Use the `## Heading` form rather than `title="..."` —
the attribute form wraps the callout in an extra `div` carrying a stray HTML
`title`, which shows up as a browser tooltip.

## Title slide options

Both are off by default, and both are title-slide only — Quarto builds that
slide from the YAML, so they are set as attributes on it rather than in the body:

```yaml
title-slide-attributes:
  data-kicker: "r-dcm"    # small mono kicker above the title
  data-atlas: "true"      # ATLAS signature; "vertical" for the stacked lockup
```

## Closing slide

A level-1 heading with `.closing`, then two columns — put a QR code or figure
in the first column and contact links in a `.end-links` block:

```markdown
# Learn more: [**r-dcm.org**](https://r-dcm.org) {.thank-you}

:::{.columns .v-center-container}
:::{.column width="60%"}
![](figure/slides-qr.png){width="50%" fig-align="center"}
:::
::: {.column .end-links width="40%"}
{{< iconify fa6-solid globe >}} \ [wjakethompson.com](https://wjakethompson.com)
:::
:::
```

## Snippets

Blockquote — hex glyph, no rule and no quotation marks:

```markdown
> Diagnostic models trade a single score for a profile of what a
> student actually knows.
```

Package sticker row:

```markdown
::: {.hex-row .large}
![](figure/measr.png) ![](figure/dcmstan.png) ![](figure/dcmdata.png)
:::
```
