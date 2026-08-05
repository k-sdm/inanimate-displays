# inanimate-displays

A playground for physical displays. Character VFDs, LED matrices and graphic
LCDs drawn to true millimetre scale from their datasheets, with typed text,
per-pixel drawing, acrylic filters, and SVG export.

Single self-contained `index.html`. No build step, no dependencies — open the
file, or serve the directory.

## Modules

### Character VFDs

| | M0220MD-202MDAR1-3 | M0220SD-202SDAR1 | M0216MD-162MDBR2-J | Futaba M202MD12BA |
|---|---|---|---|---|
| Format | 20 × 2, 5×8 | 20 × 2, 5×8 | 16 × 2, 5×8 | 20 × 2, **5×7** |
| PCB | 146 × 43 | 116 × 37 | 122 × 44 | 190 × 64 |
| Glass / display area | 130 × 33.5 | 96.9 × 25 | 108 × 31 | 146.1 × 29.0 |
| Active area | 101.75 × 18.5 | 70.75 × 11.5 | 82.7 × 19 | 146.1 × 26.0 |
| Character cell | 3.85 × 8.95 | 2.35 × 5.34 | 3.85 × 9.22 | 5.5 × 10.5 |

The first three are Newhaven. The Futaba is much larger, is the only 5 × 7 part
here, and carries a strip of 20 triangle indicator marks below the second row.

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
  the other panels pack glyphs left to right with a one-pixel gap, line 2 a
  glyph-height down when there is room.
- **Choose a font** on anything that isn't a VFD, at 1×, 2× or 3× — the same
  font-plus-integer-scale model firmware uses (`setTextSize()`).
- **Draw** by clicking or dragging across the matrix. Pixel overrides sit on top
  of the typed text and are kept per module.
- **Random fill** rolls every pixel against a density slider. *Fill* replaces
  the picture with noise, *Scatter* only ever turns pixels on so the text stays
  legible underneath, and *Re-roll breathing* re-picks the animated pixels
  without disturbing the pattern.
- **Breathing** animates a share of the lit pixels, each with its own period
  and phase so the panel shimmers rather than pulsing as one block. It survives
  export — the keyframes live inside the SVG.
- **Configure** resolution, pitch, block count and dot shape on the LED modules.
- **Filter** through Noritake's five standard acrylics, on the VFDs.
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
- **M202MD12BA dot size and pitch**, likewise, back-solved from its 5.5 × 10.5
  cell at ~75% / ~80%. Its display area is centred along the 190 mm length,
  which the sheet does not dimension, and its triangle marks are drawn below
  the second row — the sheet addresses them by the second row's digit addresses
  but never says which side of the text they sit on.
- **CharliePlex LED pitch.** Adafruit publishes only the 51.0 × 23.0 × 3.0 mm
  board. 2.80 mm is derived from fitting 15 columns inside 51 mm, and it
  reproduces 51.0 × 23.0 exactly at 15 × 7. The package body, terminals and
  emitting window are sized from photographs.
- **The LED blocks' carrier PCB**, a 3 mm margin around the tiled blocks.
- **The LCD viewing area's position** on the module, drawn centred. On a real
  part it usually sits slightly high, with the connector along the bottom.
- **The inner phosphor window inset** on the VFDs.
- **All three glyph tables**, which are drawn by hand. See below.

One result worth flagging because it is counterintuitive: the 8 × 8 blocks put
their dots just 0.35 mm inside the module edge, so butting two together leaves
the LEDs either side of a seam **3.70 mm** apart against **4.00 mm** inside a
block. Tiling compresses the seam rather than opening it up.

## Fonts

Character VFDs are hard-wired to their controller's ROM — 5 × 8 for the
Newhaven parts, 5 × 7 for the Futaba. Everything else picks a font and an
integer scale:

| | Cell | Rows needed | Notes |
|---|---|---|---|
| **3×5** | 3 × 6 | 6 | Tom Thumb proportions. The only font that clears a seven-row matrix. |
| **4×6** | 4 × 6 | 6 | Real lowercase and descenders, X11 misc-fixed idiom. |
| **5×7** | 5 × 7 | 7 | The 5 × 8 shapes squeezed into seven rows, as a 5 × 7 module renders them. |
| **5×8** | 5 × 8 | 8 | The HD44780 A00 shapes the VFD controllers carry. |

Each is full printable ASCII (0x20–0x7E, 95 glyphs) with a descender row, and
unmapped code points show a solid block the way a character ROM does. Tables
live in `G3X5`, `G4X6` and `G5X8` as rows of `#` and `.`, so a glyph can be
fixed by eye without tooling.

All three are drawn by hand rather than extracted from font files. The 5 × 8
follows the HD44780 A00 ROM and the 5 × 7 is derived from it — descending
glyphs shift up a row to land inside seven, and `j` is redrawn by hand because
its dot already occupies the top row. The 3 × 5 and 4 × 6 follow the usual
shapes for fonts of those sizes without being transcriptions of anything. Three-pixel-wide
capitals have inherent collisions — M against N in particular — which is a
property of the size, not a bug to fix.

Every panel here originally rendered 5 × 8, and that stays each one's default
and is marked `*` in the picker. The CharliePlex is the exception: at seven
rows it physically cannot hold an eight-row glyph, so it defaults to 3 × 5.

The spec panel reports how many characters and lines actually fit, and warns
when a font is too tall for the panel.

## Filters

Noritake sells five standard acrylic VFD filters — Smoke Gray (F3-05), Rose
(F3-17), Aqua (F3-16), Green (F3-12), Blue (F3-01). No transmission curves are
published, so each is modelled as a transmission colour multiplied into the
emitter plus a broadband loss: enough to show the hue shift and the contrast
gain, not a photometric prediction. They are a VFD accessory, so the control
only appears on the VFD modules.

Emitter colours come from each datasheet: CIE x=0.250 y=0.439 and x=0.235
y=0.405 for the VFDs, InGaN blue at 468 nm peak / 470 nm dominant for the LEDs.

## Licence

MIT. The datasheets themselves belong to their respective manufacturers.
