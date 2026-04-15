# RAIL CONTROL — TESTING REGIME INDEX

**Version:** v1.0

> Complete bench-test programme for the Raspberry Pi 5 control system. Eight stages, each self-contained — work through them as components arrive.
>
> **Version:** Build 10 | Last updated: April 2026

---

## 1. OVERVIEW

This testing regime validates every hardware subsystem in isolation before integration. No stage requires outdoor installation. Complete each stage before moving to the next — a failure in an early stage will cascade through later stages and waste time debugging.

The test order matches the physical dependency chain: you cannot test LED drivers (Stage 3) without confirmed MCP23017 outputs (Stage 2), and you cannot run the integration soak (Stage 6) without all subsystems proven individually.

---

## 2. STAGES AT A GLANCE

| Stage | File | Subsystem | Components | Duration |
|---|---|---|---|---|
| **0** | `TESTING-01-STAGE-0.md` | Pi baseline | OS, I²C, GPIO, Python | 30 min |
| **1** | `TESTING-02-STAGE-1.md` | I²C & detection | TXB0104, MCP23017 ×3 | 20 min |
| **2** | `TESTING-03-STAGE-2.md` | MCP outputs | 48 GPIO pins | 45 min |
| **3** | `TESTING-04-STAGE-3.md` | LED drivers | BC547, resistors, 12V | 60 min |
| **4** | `TESTING-05-STAGE-4.md` | Relay & turnout | 8-ch relay, LGB motor, 18V AC | 45 min |
| **5** | `TESTING-06-STAGE-5.md` | Hall sensors | KY-024 modules, magnets | 40 min |
| **6** | `TESTING-07-STAGE-6.md` | Integration soak | All subsystems combined | 30 min+ |
| **7** | `TESTING-08-STAGE-7.md` | Stripboard & enclosure | Soldering, mechanical fit | 2–3 hours |

---

## 3. DEFERRED TESTING

The following components are not tested in this regime and depend on track installation (Phase 9+):

- **BTS7960 H-bridge** — traction PWM control
- **ADS1115 ADC** — current sense on traction rails

A separate testing regime will be created once track layout is complete.

---

## 4. EQUIPMENT CHECKLIST

Everything you need across all eight stages. Tick off as confirmed available.

**Core Hardware**
- [ ] Raspberry Pi 5 + USB-C PD PSU (27W / 5A minimum)
- [ ] MicroSD card (32GB+) with Raspberry Pi OS Bookworm 64-bit
- [ ] Keyboard, monitor, or SSH access

**I²C & GPIO Expansion**
- [ ] TXB0104 level shifter module
- [ ] CJMCU-2317 (MCP23017) breakout boards × 3
- [ ] 40-pin jumper wires, breadboard

**LED Driver Components**
- [ ] BC547 NPN transistors (TO-92) — at least 10
- [ ] 1kΩ resistors ×10 (base resistors)
- [ ] 560Ω resistors ×10 (red/yellow/white LEDs)
- [ ] 680Ω resistors ×4 (green LEDs)
- [ ] 330Ω resistor ×1 (GPIO sanity check, Stage 0)
- [ ] 3mm LEDs: red, yellow/amber, green, warm white — at least 3 of each

**Power Supplies**
- [ ] 12V DC supply (2A minimum for LEDs)
- [ ] 5V DC supply (2A minimum for Pi & logic)
- [ ] LGB transformer (18V AC) — existing layout supply

**Relay & Turnout Testing**
- [ ] 8-channel relay module (5V, optoisolated)
- [ ] LGB turnout motor × 1 (bench test)
- [ ] 2A fuse + PCB fuse holder

**Hall Sensors**
- [ ] KY-024 Hall effect sensor modules ×5 minimum
- [ ] Neodymium magnets (the ones for rolling stock)

**Tools & Measurement**
- [ ] Multimeter (voltage, current, continuity)
- [ ] Ruler or callipers (sensor distance measurement)
- [ ] Soldering iron + solder + flux (Stage 7)
- [ ] Stripboard (9 × 15 cm) ×3
- [ ] 20AWG solid core UL1007 wire (multicolour, internal only)
- [ ] 22AWG stranded wire (external cable runs)
- [ ] Screw terminals (2-pin, 5mm pitch) ×10
- [ ] IC sockets (28-pin DIP) ×3 (for MCP23017, Stage 7)
- [ ] IP65 enclosure
- [ ] IP68 cable glands (PG7) ×8

---

## 5. HOW TO USE THESE DOCUMENTS

Each stage file is structured identically:

1. **Goal** — what you are testing and why
2. **Equipment** — what you need for this stage
3. **Wiring** — exact pin connections (where applicable)
4. **Tests** — numbered tests with checkboxes for tracking progress
5. **Pass Criteria** — requirements before moving to the next stage
6. **Measured Values** — table for recording real data from your build

**Workflow:**

- Print each stage when ready to work on it
- Tick checkboxes as you progress
- Record measured values — these become your reference for debugging
- If a test fails, consult the troubleshooting table in Stage 7

> 💡 **Pro tip:** Use a pencil to tick checkboxes. Pencil marks are easier to correct if you need to repeat a test.

---

## 6. TIMELINE

Working through all eight stages takes approximately **5–7 hours** of active testing time, spread over multiple sessions.

| Stage | Time | Notes |
|---|---|---|
| 0 | 30 min | Can do immediately — no hardware needed |
| 1 | 20 min | Requires breadboard + TXB0104 + MCP23017 boards |
| 2 | 45 min | Methodical — 48 pins to test |
| 3 | 60 min | Daylight testing required for LED visibility check |
| 4 | 45 min | Real motor testing — fun stage |
| 5 | 40 min | Sensor tuning may take iteration |
| 6 | 30 min+ | Soak test runs while you do other things |
| 7 | 2–3 hours | Soldering + mechanical fit — takes patience |

Start with Stage 0 immediately (no components needed). Proceed to subsequent stages as hardware arrives.

---

## 7. CHAPTER 1 STRUCTURE

This testing regime is **Chapter 1** of the larger G Scale Garden Railway Control System manual. Subsequent chapters cover:

- **Chapter 2:** Hardware Design & Architecture
- **Chapter 3:** Firmware & Software Setup
- **Chapter 4:** Browser Control Panel
- **Chapter 5:** Outdoor Installation & Weatherproofing
- **Chapter 6:** Troubleshooting & Maintenance

---

## LICENCE

This project is released under the **GNU GPL v3**. You are free to use, modify, and distribute these documents and the hardware designs. All derivative works must also be released under GNU GPL v3.

For full details, see the `LICENCE` file or visit https://www.gnu.org/licenses/gpl-3.0.html

---

*Tsana Forge — RAIL CONTROL | G Scale Garden Railway Control System | v1.0 | https://tsana.net*
