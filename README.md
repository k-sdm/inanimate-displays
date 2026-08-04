# inanimate-displays

A playground for character VFD modules. Three Newhaven parts drawn to true
millimetre scale from their mechanical drawings, with typed text, per-pixel
drawing, Noritake acrylic filters, and SVG export.

Single self-contained `index.html`. No build step, no dependencies — open the
file, or serve the directory.

## Modules

| | M0220MD-202MDAR1-3 | M0220SD-202SDAR1 | M0216MD-162MDBR2-J |
|---|---|---|---|
| Format | 20 × 2 | 20 × 2 | 16 × 2 |
| PCB | 146 × 43 | 116 × 37 | 122 × 44 |
| VFD glass | 130 × 33.5 | 96.9 × 25 | 108 × 31 |
| Active area | 101.75 × 18.5 | 70.75 × 11.5 | 82.7 × 19 |
| Character cell | 3.85 × 8.95 | 2.35 × 5.34 | 3.85 × 9.22 |
| Active / PCB | 30.0% | 19.0% | 29.3% |

All dimensions in mm. Every module renders at the same mm-per-pixel, so their
real relative sizes are directly comparable.

## Use

- **Type** into the two line inputs. Overflow past the column count is flagged.
- **Draw** by clicking or dragging across the dot matrix. Pixel overrides sit on
  top of the typed text and are kept per module.
- **Filter** through Noritake's five standard acrylics.
- **Dimensions** overlays the datasheet callouts on the drawing.
- **Export SVG** writes the current state out at real size (`146mm × 43mm` etc.),
  with the interaction layer stripped.

## Where the numbers come from

Dimensions are the callouts on each datasheet's mechanical drawing and
specification tables. The datasheets are vector PDFs with no extractable
dimension text, so callouts were read from rendered pages and then cross-checked
by measuring the drawings' own vector geometry.

Each module's numbers close on themselves: dot size and pitch reconstruct the
character cell exactly, and character and line pitch reconstruct the active area
exactly.

Things that are *not* straight off a datasheet, listed in-app per module:

- **M0220SD dot size and pitch.** That datasheet never states them. Back-solved
  from the 5 × 8 cell at an assumed ~70% / ~78% fill.
- **M0216MD character pitch.** The panel table's "3.85 × 8.03" contradicts its
  own display size; measuring the drawing's 1280 dots gives ~5.24 × 10.24, so
  pitch is derived from the 82.7 mm span instead.
- **The inner phosphor window inset**, eyeballed on all three.
- **Glyphs** are hand-transcribed HD44780 A00 shapes, not a factory ROM dump.
  They live in the `GLYPHS` table as 8 rows of 5 columns and are easy to fix
  one at a time.

## Filters

Noritake sells five standard acrylic VFD filters — Smoke Gray (F3-05), Rose
(F3-17), Aqua (F3-16), Green (F3-12), Blue (F3-01). No transmission curves are
published, so each is modelled here as a transmission colour multiplied into the
emitter plus a broadband loss: enough to show the hue shift and the contrast
gain, not a photometric prediction.

Emitter colours are derived from each datasheet's own CIE coordinates
(x=0.250 y=0.439 for the M0220MD, x=0.235 y=0.405 for the M0220SD).

## Licence

MIT. The datasheets themselves belong to Newhaven Display International.
