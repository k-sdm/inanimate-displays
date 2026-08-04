# inanimate-displays

A playground for physical displays. Character VFDs and LED matrices drawn to
true millimetre scale from their datasheets, with typed text, per-pixel drawing,
acrylic filters, and SVG export.

Single self-contained `index.html`. No build step, no dependencies — open the
file, or serve the directory.

## Modules

### Character VFDs (Newhaven)

| | M0220MD-202MDAR1-3 | M0220SD-202SDAR1 | M0216MD-162MDBR2-J |
|---|---|---|---|
| Format | 20 × 2 | 20 × 2 | 16 × 2 |
| PCB | 146 × 43 | 116 × 37 | 122 × 44 |
| VFD glass | 130 × 33.5 | 96.9 × 25 | 108 × 31 |
| Active area | 101.75 × 18.5 | 70.75 × 11.5 | 82.7 × 19 |
| Character cell | 3.85 × 8.95 | 2.35 × 5.34 | 3.85 × 9.22 |
| Active / PCB | 30.0% | 19.0% | 29.3% |

### LED matrices

**Adafruit 15×7 CharliePlex FeatherWing** — 2020 packages set at 45°, so each
LED presents its 2.83 mm diagonal in both axes. Stock is 15 × 7 on a 2.80 mm
pitch inside a 51.0 × 23.0 mm board, which puts the diamonds just touching.
Columns, rows and pitch are all editable; the board keeps the stock edge margins
and grows around the matrix.

Drawn as physical parts rather than lit cells: every package body is visible
whether or not it is powered, with a smaller rounded lens inside it that carries
the light. Unpowered lenses still catch ambient light, so they read as pale
grey — the "unlit dots" toggle controls them. Lit LEDs saturate to white at the
centre and bloom harder than a VFD phosphor.

**Luckylight KWM-R30881XBB** — 1.2" 8 × 8 blocks, 31.7 mm square, 3.0 mm dots on
a 4.0 mm pitch. Tile as many as you like across and down. Square or round dots.

All dimensions in mm. Every module renders at the same mm-per-pixel, so their
real relative sizes are directly comparable.

## Use

- **Type** into the two line inputs. VFDs place one glyph per character cell;
  LED matrices pack 5 × 8 glyphs left to right with a one-pixel gap, line 2
  eight rows down when there is room.
- **Draw** by clicking or dragging across the matrix. Pixel overrides sit on top
  of the typed text and are kept per module.
- **Configure** resolution, pitch, block count and dot shape on the LED modules.
- **Filter** through Noritake's five standard acrylics.
- **Dimensions** overlays the datasheet callouts on the drawing.
- **Export SVG** writes the current state out unitless at **1 px = 1 mm**, with
  the interaction layer stripped.

## Where the numbers come from

Dimensions are the callouts on each part's datasheet or mechanical drawing. The
Newhaven datasheets are vector PDFs with no extractable dimension text, so
callouts were read from rendered pages and then cross-checked by measuring the
drawings' own vector geometry.

Each module's numbers close on themselves: dot size and pitch reconstruct the
character cell or block exactly, and the pixel grid reconstructs the active area
exactly.

Things that are *not* straight off a datasheet, listed in-app per module:

- **M0220SD dot size and pitch.** Never stated. Back-solved from the 5 × 8 cell
  at an assumed ~70% / ~78% fill.
- **M0216MD character pitch.** The panel table's "3.85 × 8.03" contradicts its
  own display size; measuring the drawing's 1280 dots gives ~5.24 × 10.24, so
  pitch is derived from the 82.7 mm span instead.
- **CharlieWing LED pitch.** Adafruit publishes only the 51.0 × 23.0 × 3.0 mm
  board. 2.80 mm is derived from fitting 15 columns of 45° 2020 packages inside
  51 mm, and it reproduces 51.0 × 23.0 exactly at the stock 15 × 7.
- **The LED blocks' carrier PCB**, a 3 mm margin around the tiled blocks. That
  is whatever you design. The blocks themselves are to spec.
- **The inner phosphor window inset** on the VFDs.
- **Glyphs** are hand-transcribed HD44780 A00 shapes, not a factory ROM dump.
  They live in the `GLYPHS` table as 8 rows of 5 columns and are easy to fix one
  at a time.

One result worth flagging because it is counterintuitive: the 8 × 8 blocks put
their dots just 0.35 mm inside the module edge, so butting two together leaves
the LEDs either side of a seam **3.70 mm** apart against **4.00 mm** inside a
block. Tiling compresses the seam rather than opening it up. Drawn as it falls
out of the numbers.

## Filters

Noritake sells five standard acrylic VFD filters — Smoke Gray (F3-05), Rose
(F3-17), Aqua (F3-16), Green (F3-12), Blue (F3-01). No transmission curves are
published, so each is modelled as a transmission colour multiplied into the
emitter plus a broadband loss: enough to show the hue shift and the contrast
gain, not a photometric prediction. They apply to the LED matrices too, where
the same physics holds for any acrylic contrast filter.

Emitter colours come from each datasheet: CIE x=0.250 y=0.439 and x=0.235
y=0.405 for the VFDs, InGaN blue at 468 nm peak / 470 nm dominant for the LEDs.

## Licence

MIT. The datasheets themselves belong to their respective manufacturers.
