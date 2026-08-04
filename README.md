# inanimate-displays

A playground for physical displays. Character VFDs, LED matrices and graphic
LCDs drawn to true millimetre scale from their datasheets, with typed text,
per-pixel drawing, acrylic filters, and SVG export.

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

### Custom Charlieplexed LED Matrix

Discrete rectangular chip LEDs laid at 45°, modelled on the Adafruit 15 × 7
CharliePlex FeatherWing. Stock is 15 × 7 on a 2.80 mm pitch inside a
51.0 × 23.0 mm board. Columns, rows and pitch are editable; the board keeps the
stock edge margins and grows around the matrix.

Each LED is drawn as a real part — a 2.00 × 1.25 mm body with a solder terminal
at each end and an emitting window in the middle — not as a lit cell. The
packages are visible whether or not they are powered.

### Luckylight KWM-R30881XBB

1.2" 8 × 8 blocks, 31.7 mm square, 3.0 mm dots on a 4.0 mm pitch. Tile as many
as you like across and down. Square or round dots.

### Futurlec BLUE128X64LCD

128 × 64 graphic LCD, white pixels on a blue LED backlight. Module 93.0 × 70.0,
viewing area 62.0 × 33.0, dot 0.42 × 0.48. Negative mode, so the backlight is
the background and pixels switch white — unlit pixels are the faint grid you can
see on a real panel, not black.

All dimensions in mm. Every module renders at the same mm-per-pixel, so their
real relative sizes are directly comparable.

## Use

- **Type** into the two line inputs. VFDs place one glyph per character cell;
  the other panels pack 5 × 8 glyphs left to right with a one-pixel gap, line 2
  eight rows down when there is room.
- **Draw** by clicking or dragging across the matrix. Pixel overrides sit on top
  of the typed text and are kept per module.
- **Configure** resolution, pitch, block count and dot shape on the LED modules.
- **Filter** through Noritake's five standard acrylics.
- **Dimensions** overlays the datasheet callouts on the drawing.
- **Export SVG** writes the current state out unitless at **1 px = 1 mm**.

## Where the numbers come from

Dimensions are the callouts on each part's datasheet or mechanical drawing. The
Newhaven datasheets are vector PDFs with no extractable dimension text, so
callouts were read from rendered pages and then cross-checked by measuring the
drawings' own vector geometry.

Every module's numbers close on themselves: the pixel grid reconstructs the
stated active area exactly, and the active area sits inside the outline.

Two datasheets contradict themselves, and in both cases the conflict is resolved
in favour of the dimension that is geometrically possible:

- **M0216MD.** The panel table's "Character Pitch 3.85 × 8.03" cannot produce
  its own 82.7 mm display size. Measuring the drawing's 1280 dots gives
  ~5.24 × 10.24, so pitch is derived from the 82.7 mm span.
- **BLUE128X64LCD.** The table's "Dot Pitch 0.52 × 0.52" cannot fit a 62.0 mm
  viewing area — 128 × 0.52 is 66.56 mm. Pitch is derived from the viewing area
  instead, giving 0.485 × 0.516, close to the 0.48 × 0.52 this module family
  normally quotes.

Things that are *not* on a datasheet at all, listed in-app per module:

- **M0220SD dot size and pitch.** Never stated. Back-solved from the 5 × 8 cell
  at an assumed ~70% / ~78% fill.
- **CharliePlex LED pitch.** Adafruit publishes only the 51.0 × 23.0 × 3.0 mm
  board. 2.80 mm is derived from fitting 15 columns inside 51 mm, and it
  reproduces 51.0 × 23.0 exactly at 15 × 7. The package body, terminals and
  emitting window are sized from photographs.
- **The LED blocks' carrier PCB**, a 3 mm margin around the tiled blocks.
- **The LCD viewing area's position** on the module, drawn centred. On a real
  part it usually sits slightly high, with the connector along the bottom.
- **The inner phosphor window inset** on the VFDs.
- **Glyphs** are hand-transcribed HD44780 A00 shapes, not a factory ROM dump.

One result worth flagging because it is counterintuitive: the 8 × 8 blocks put
their dots just 0.35 mm inside the module edge, so butting two together leaves
the LEDs either side of a seam **3.70 mm** apart against **4.00 mm** inside a
block. Tiling compresses the seam rather than opening it up.

## Filters

Noritake sells five standard acrylic VFD filters — Smoke Gray (F3-05), Rose
(F3-17), Aqua (F3-16), Green (F3-12), Blue (F3-01). No transmission curves are
published, so each is modelled as a transmission colour multiplied into the
emitter plus a broadband loss: enough to show the hue shift and the contrast
gain, not a photometric prediction. They apply to the other panels too, where
the same physics holds for any acrylic contrast filter.

Emitter colours come from each datasheet: CIE x=0.250 y=0.439 and x=0.235
y=0.405 for the VFDs, InGaN blue at 468 nm peak / 470 nm dominant for the LEDs.

## Licence

MIT. The datasheets themselves belong to their respective manufacturers.
