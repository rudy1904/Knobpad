# Project-local KiCad library (`knob`)

Footprints/symbols that are NOT in stock KiCad 10 live here, so the repo opens on any machine without extra installs.

| File | What | Source / licence |
| --- | --- | --- |
| `knob.pretty/SW_Kailh_Choc_V1_1.00u.kicad_mod` | Kailh Choc V1 (PG1350), soldered, 1u outline | Copied unchanged from [kiswitch/keyswitch-kicad-library](https://github.com/kiswitch/keyswitch-kicad-library) `library/footprints/Switch_Keyboard_Kailh.pretty/`, commit `9edbd5e`. Dual licensed MIT / CC-BY-SA 4.0 (with the KiCad-style design exception). |
| `knob.pretty/LOLIN_S3_Mini.kicad_mod` | LOLIN S3 Mini, 4 × 8 pins, module top view (place on B.Cu) | Drawn from Wemos dim_s3_mini_v1.0.0: outer rows ±11.43 mm, inner ±8.89 mm, first row 7.7 mm from antenna end. Pads 1–8 left-outer, 9–16 left-inner, 17–24 right-inner, 25–32 right-outer (top → bottom). |
| `knob.kicad_sym` | `LOLIN_S3_Mini` symbol, 32 pins in physical row order | Pinout from Wemos schematic v1.0.0 + CircuitPython `lolin_s3_mini` board file |

Note: the Choc footprint's 3D model path points at the kiswitch PCM install (`${KICAD6_3RD_PARTY}`); the 3D view only shows the switch if that library is installed through KiCad's Plugin & Content Manager. Electrically it doesn't matter.

Already registered in the project's `sym-lib-table` / `fp-lib-table` as nickname `knob` (`${KIPRJMOD}/lib/...`).
