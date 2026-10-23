# Knob (panda MIDI keypad) — parts draft

Prices/stock checked on LCSC on 2026-10-22. "est." = NOT checked, placeholder only.
This is the working list. Half Life generates and overwrites `BOM.md` on sync (same as Hushlog), so enter the final list on the platform; don't hand-edit `BOM.md`.

| Ref | Part | MPN | Source | Buy qty | Unit $ | Line $ | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| U1 | LOLIN S3 Mini (ESP32-S3FH4R2) | — | Wemos official store; not on LCSC | 1 | est. 5 | est. 5 | |
| SW1–SW8 | Kailh Choc V1 switch | CPG1350xx (pick feel) | **Not available on LCSC** (all Kailh Choc listings "Not available now") — Kailh store / Indian keyboard shop | 10 (2 spare) | est. 0.6 | est. 6 | |
| — | Choc V1 1u keycaps | — | same as switches | 8 | est. 0.6 | est. 5 | Must be Choc-stem; MX keycaps don't fit V1 |
| SW9 | Encoder + push, 36 det / 18 pulses | EC11E18244A5 | [LCSC C255515](https://www.lcsc.com/product-detail/C255515.html) | 1 | 2.4355 | 2.44 | 284 in stock |
| RV1 | 10k linear pot, 9 mm, shaft up, bushingless, 20 mm flat shaft | PTV09A-4020F-B103 | [LCSC C5848782](https://www.lcsc.com/product-detail/C5848782.html) | 1 | 0.5822 | 0.58 | 200 in stock, other-supplier stock, 5–7 day lead |
| D1–D8 | Addressable RGB LED 5050 | WS2812B-B/T | [LCSC C2761795](https://www.lcsc.com/product-detail/C2761795.html) | 10 (MOQ 5) | 0.1182 | 1.18 | 20,895 in stock; LCSC marks it Discontinued — fine for one build |
| U2 | Buffer, TTL-level input (3.3 V → 5 V data) | SN74AHCT1G125DBVR | [LCSC C7484](https://www.lcsc.com/product-detail/C7484.html) | 5 (MOQ 5) | 0.1011 | 0.51 | Only if decision D1 = yes |
| R1–R4 | 10 kΩ 0805 1% | 0805W8F1002T5E | [LCSC C17414](https://www.lcsc.com/product-detail/C17414.html) | 100 | 0.0034 | 0.34 | Encoder: 2 pull-ups + 2 series |
| R5 | 470 Ω 0805 1% | 0805W8F4700T5E | [LCSC C17710](https://www.lcsc.com/product-detail/C17710.html) | 100 | 0.0049 | 0.49 | LED data series resistor |
| C1–C2 | 10 nF 0805 X7R 50 V | CC0805KRX7R9BB103 | [LCSC C83170](https://www.lcsc.com/product-detail/C83170.html) | 100 | 0.0082 | 0.82 | Encoder filter. (C1710, the usual pick, is out of stock) |
| C3–C13 | 100 nF 0805 X7R 50 V | CC0805KRX7R9BB104 | [LCSC C49678](https://www.lcsc.com/product-detail/C49678.html) | 20 | 0.0206 | 0.41 | 1 per LED (8) + pot + 3V3 + U2 |
| C14 | 10 µF 0805 X5R 25 V | CL21A106KAYNNNE | [LCSC C15850](https://www.lcsc.com/product-detail/C15850.html) | 20 | 0.0429 | 0.86 | Bulk at LED chain (replaces the plan's 1000 µF) |
| — | Knobs for EC11 + PTV09 shafts | — | any | 2 | est. 0.5 | est. 1 | Match the shaft type in each datasheet |
| — | M2 standoffs/feet + screws | — | any | 4 sets | est. 0.25 | est. 1 | Height must clear the module on the back |
| — | PCB 2-layer, ≤100 × 100 mm, 5 pcs | — | JLCPCB | 1 | est. 2–5 | est. 2–5 | + shipping |

**LCSC lines (verified): $7.63** (with U2). Estimated other lines: ~$20–23. **Total ≈ $28–31 before shipping/customs** — right at the $30 target, so the switch/keycap/module prices decide it. LCSC shipping to India is not included; Half Life's site says customs aren't covered.
