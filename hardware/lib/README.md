# Project-local KiCad library (`knob`)

Footprints/symbols that are NOT in stock KiCad 10 live here, so the repo opens on any machine without extra installs.

| File | What | Source / licence |
| --- | --- | --- |
| `knob.pretty/SW_Kailh_Choc_V1_1.00u.kicad_mod` | Kailh Choc V1 (PG1350), soldered, 1u outline | Copied unchanged from [kiswitch/keyswitch-kicad-library](https://github.com/kiswitch/keyswitch-kicad-library) `library/footprints/Switch_Keyboard_Kailh.pretty/`, commit `9edbd5e`. Dual licensed MIT / CC-BY-SA 4.0 (with the KiCad-style design exception). |
| `knob.pretty/LOLIN_S3_Mini.kicad_mod` | LOLIN S3 Mini, 4 x 8 pins | TODO (Phase 2/3) — drawn from Wemos dimension drawing v1.0.0 |
| `knob.kicad_sym` | LOLIN S3 Mini symbol | TODO (Phase 2) |

Note: the Choc footprint's 3D model path points at the kiswitch PCM install (`${KICAD6_3RD_PARTY}`); the 3D view only shows the switch if that library is installed through KiCad's Plugin & Content Manager. Electrically it doesn't matter.

Register in the KiCad project (Phase 2): Preferences -> Manage Footprint Libraries -> Project Specific -> nickname `knob`, path `${KIPRJMOD}/lib/knob.pretty`.
