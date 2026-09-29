# A7670E carrier mechanical footprint

Library ID: `WeatherStation:A7670E_Module_53x32_M2_Mounting_REFERENCE`.

The user specified a **53 x 32 mm module body**, **M2 mounting screws**, and
**no electrical pins**. The footprint is a reusable mechanical-only library
item, now added as **M1** in `STM32_Phil.kicad_pcb` at **(148, 68) mm**.
It is staged to the right of the board outline for placement; final layout
within the board remains to be arranged. Existing footprints are unchanged.

- Body: 32 mm along X, 53 mm along Y; footprint origin at the body center.
- Four unnumbered, non-plated holes (NPTH), diameter 2.2 mm.
- Hole type follows the installed KiCad `MountingHole_2.2mm_M2` reference.
- No electrical pads, copper annuli, nets or schematic symbol.
- Excluded from BOM and placement files.

## Provisional mounting pattern

The photo does not specify hole-center coordinates. **2.5 mm insets from
each body edge are an assumption**, giving **27 x 48 mm center spacing**:

| Corner | X from center (mm) | Y from center (mm) |
| --- | --- | --- |
| Top left | -13.5 | -24 |
| Top right | 13.5 | -24 |
| Bottom left | -13.5 | 24 |
| Bottom right | 13.5 | 24 |

Measure the actual module before using this hole pattern for fabrication.
The comments layer shows the same nominal 4.4 mm diameter mounting area as
the installed KiCad M2 mounting-hole footprint. Screw heads, spacers, module
height and connector overhang are not specified by the supplied photo.

The `F.Fab` and `F.SilkS` rectangles show the module body. They are not
Edge.Cuts and do not cut an opening in the carrier PCB. J6 remains the
existing electrical connector for the A7670E carrier.
