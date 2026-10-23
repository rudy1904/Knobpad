# Firmware (Phase 9)

Not started. Target: CircuitPython on the LOLIN S3 Mini, native USB-MIDI.

- 8 keys -> MIDI notes (internal pull-ups, active low)
- EC11 -> MIDI CC (EC11E18244A5 is 36 detents / 18 pulses: one click = half a quadrature cycle, so divisor = 2)
- Pot -> MIDI CC (ADC1, GPIO1)
- WS2812B underglow (cap brightness; see docs/phase1-prep.md)

Decision pending: KMK vs a plain CircuitPython script (see docs/phase1-prep.md, D4).
Flash CircuitPython onto the module BEFORE soldering it to the back of the board.
