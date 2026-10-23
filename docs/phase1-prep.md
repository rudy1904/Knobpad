# Phase 1 — Prep (done 2026-10-22, waiting for review)

**What we're building:** a panda-shaped USB-MIDI controller. 8 Kailh Choc V1 keys (2 ears, 2 eyes, nose, mouth, 2 cheeks) → MIDI notes; EC11 encoder with push → CC; 10k rotary pot → CC; WS2812B underglow around the outline; LOLIN S3 Mini (ESP32-S3, native USB) on the back. 2-layer PCB, KiCad 10, Half Life Tier 1.

KiCad version confirmed: your Hushlog files were saved by KiCad 10.0. Footprint names below were checked against the official library at tag `10.0.7`.

---

## 1. Where the plan was wrong

| # | Plan said | Verified reality | What changes |
| --- | --- | --- | --- |
| 1 | "~16 usable GPIOs" | 27 GPIOs on 32 pins, in **4 rows** (two 2×8 blocks) | More headroom than planned |
| 2 | "2×8 header footprint" | The outer 2 rows (D1-mini compatible) carry only 12 GPIOs, 2 of them UART0. We need 13. | Footprint must include all 4 rows. Custom footprint (Phase 2/3) |
| 3 | Avoid strapping pins GPIO0/45/46 | GPIO45/46 aren't broken out at all; GPIO0 is only the onboard BOOT button. The strapping pin that IS on the header is **GPIO3** | Don't use GPIO3 |
| 4 | `Button_Switch_Keyboard:SW_Kailh_Choc_V1_1.00u` | Doesn't exist in stock KiCad 10.0.7 (that library is Cherry MX + Matias only). Name comes from the third-party kiswitch library | Vendored into `hardware/lib/knob.pretty/` |
| 5 | 1000 µF bulk cap | Wrong for a USB-powered board: big inrush at plug-in, tall can on a pretty board. Module already has 10 µF on VBUS | 10 µF 0805 near the first LED + 100 nF per LED |
| 6 | Encoder RC: 10k pull-up + 0.1 µF to GND | Puts 100 nF straight across the encoder contacts (dumped on every click) | Bourns' standard filter: 10k pull-up, **10k series**, **10 nF** to GND, per channel |
| 7 | WS2812B data straight from GPIO | Depends on the exact LED. WS2812B-B/T (C2761795) datasheet: VIH min 2.7 V at VDD 5 V → 3.3 V is in spec. Older/generic WS2812B datasheets: VIH = 0.7·VDD = 3.5 V → 3.3 V is **out** of spec | Decision D1 |
| 8 | EC11 footprint `RotaryEncoder_Alps_EC11E` | That one has no push-switch pins | `Rotary_Encoder:RotaryEncoder_Alps_EC11E-Switch_Vertical_H20mm` |
| 9 | Put the BOM in `BOM.md` | Half Life syncs and **overwrites** `BOM.md` and `JOURNAL.md` (see Hushlog) | Draft list is `docs/parts.md`; final list goes on the platform |
| 10 | Pick switches on LCSC | Every Kailh Choc listing on LCSC (switches and hotswap sockets) is "Not available now" | Buy switches + keycaps elsewhere; solder them (no hotswap) |

## 2. LOLIN S3 Mini — verified facts

Sources: Wemos pinout image + schematic v1.0.0 + dimension drawing v1.0.0, CircuitPython `lolin_s3_mini/pins.c`.

- ESP32-S3FH4R2: 4 MB flash, 2 MB **quad** PSRAM in-package → GPIO33–37 are free (they're only taken on octal-PSRAM parts).
- Board 25.4 × 34.3 mm. 4 rows × 8 pins, 2.54 mm pitch. Outer rows 22.86 mm apart (1.27 mm from each long edge), inner rows 17.78 mm apart. First pin row 7.7 mm from the antenna end, last row 8.8 mm from the USB end. Two Ø2.0 mm holes at the antenna end, 2.5 mm in from each side.
- **VBUS pin = USB-C VBUS directly** (no diode, no fuse).
- 3V3 from an ME6211C33 LDO (500 mA class), 10 µF in and out.
- Onboard: RGB LED on GPIO47 (not on the header), BOOT = GPIO0, RESET = EN.

Header, top view (antenna up, USB-C down):

| Pos | Left outer | Left inner | Right inner | Right outer |
| --- | --- | --- | --- | --- |
| 1 | EN | IO1 | IO33 | IO43 (TX) |
| 2 | IO2 | IO3 ⚠ strap | IO37 | IO44 (RX) |
| 3 | IO4 | IO5 | IO38 | IO36 (SCL) |
| 4 | IO12 | IO6 | IO34 | IO35 (SDA) |
| 5 | IO13 | IO7 | IO21 | IO18 |
| 6 | IO11 | IO8 | IO17 | IO16 |
| 7 | IO10 | IO9 | GND | GND |
| 8 | 3V3 | IO14 | IO15 | VBUS |

ADC1 = GPIO1–10 (pot must go here; ADC2 = GPIO11–20 conflicts with Wi-Fi).

## 3. Pin map (proposed — final swap happens at placement, Phase 5)

| Signal | GPIO | Header pos | Why |
| --- | --- | --- | --- |
| POT_WIPER | IO1 | L-inner 1 | ADC1_CH0 |
| ENC_A | IO12 | L-outer 4 | 3 encoder pins adjacent |
| ENC_B | IO13 | L-outer 5 | |
| ENC_SW | IO11 | L-outer 6 | internal pull-up |
| LED_DIN | IO16 | R-outer 6 | sits next to GND + VBUS, so LED power and data leave together |
| KEY1–KEY8 | IO33, IO37, IO38, IO34, IO21, IO17, IO15, IO18 | right side | one side of the module, internal pull-ups, active low |

13 of 27 used. Avoid: IO3 (strapping), IO43/44 (UART0 console). Keep free: IO35/36 (I2C, for an OLED later). Spare: IO2, 4–10, 14 — all ADC1, so a fader can be added later.
The module sits on the back, so its footprint gets flipped to B.Cu — the pin columns mirror left/right as seen from the front. Digital pins are interchangeable, so we re-assign keys at placement to untangle the ratsnest.

## 4. Footprints (checked against KiCad library tag 10.0.7)

| Part | Footprint | Status |
| --- | --- | --- |
| Choc V1 | `knob:SW_Kailh_Choc_V1_1.00u` | vendored (kiswitch, MIT/CC-BY-SA) |
| EC11 w/ switch | `Rotary_Encoder:RotaryEncoder_Alps_EC11E-Switch_Vertical_H20mm` | stock |
| WS2812B 5050 | `LED_SMD:LED_WS2812B_PLCC4_5.0x5.0mm_P3.2mm` | stock |
| PTV09A-4020F pot | `Potentiometer_THT:Potentiometer_Bourns_PTV09A-1_Single_Vertical` | stock — Bourns' naming is inverted ("-4 = horizontal/rear mount" means shaft up). Check pins vs the -4 drawing in Phase 3 |
| 74AHCT1G125 | `Package_TO_SOT_SMD:SOT-23-5` | stock |
| R / C | `Resistor_SMD:R_0805_2012Metric`, `Capacitor_SMD:C_0805_2012Metric` | stock |
| LOLIN S3 Mini | `knob:LOLIN_S3_Mini` | custom, to draw from the dimensions above |

## 5. Mechanical / build gotchas

- Module on the back ⇒ the board stands on the module. You need standoffs taller than the module stack (headers + module + USB-C). Underglow needs that gap anyway.
- Flash CircuitPython onto the module **before** soldering it; its BOOT/RESET buttons will face the desk.
- The module's USB-C end must sit flush with the board edge.
- Keep the board ≤ 100 × 100 mm to stay in JLCPCB's cheapest tier. 8 Choc keys (18 × 17 mm spacing) + encoder + pot + ears will be tight — plan a ~90–95 mm head.
- LED current: WS2812B-B/T is tested at 12 mA/channel ≈ 36 mA full white per LED → 8 LEDs ≈ 0.3 A plus the ESP32. Cap brightness in firmware.
- EC11E18244A5: 36 detents / 18 pulses = half a quadrature cycle per click → divisor 2 in firmware.

## 6. Decisions needed before Phase 2

- **D1 — LED level shifter.** Add 74AHCT1G125 (C7484, $0.10, one SOT-23-5): works with any WS2812B you end up buying. Skip it: only safe if you actually buy WS2812B-B/T (or another variant specced VIH ≤ 3.0 V). Recommendation: add it.
- **D2 — Who draws the schematic/PCB (Half Life hours).** Half Life counts journal + Hackatime hours and uses your timelapse as evidence against claimed time. If I draw it, those hours aren't yours to log and the timelapse won't show them. Options: (a) I give a block-by-block spec + library parts, you draw, I review the file; (b) I draw it and you don't log those phases.
- **D3 — Switches soldered, not hotswap.** Hotswap sockets are also unavailable at LCSC. (Recommendation: soldered.)
- **D4 — Firmware (can wait).** KMK is keyboard firmware; for a MIDI-only device a plain CircuitPython script (usb_midi + rotaryio + analogio + neopixel) is simpler and easier to debug.

## Sources
See `docs/ref/README.md`.
