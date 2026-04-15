# RAIL STACK
 
> A Raspberry Pi 5–based automation system for G scale (LGB) garden railway signals, street lighting, and turnouts. Designed for outdoor installation with comprehensive documentation aimed at hobbyists and railway enthusiasts who want to automate their layouts.
 
---
 
## Overview
 
**RAIL STACK** is an open-source hardware and software project that brings automation and live control to garden-scale model railway layouts. Using a Raspberry Pi 5 as the central controller, the system manages up to 48 LED signal outputs, 6 turnout motors, and block detection via Hall effect sensors — all suitable for long outdoor cable runs in harsh garden environments.
 
The project is distinguished by its **exceptional documentation**. Every component, procedure, and decision has been documented in plain language with practical examples, designed to be accessible to both experienced electronics enthusiasts and those new to Raspberry Pi projects. The goal is not just to publish working code, but to create a **complete guide** that others can follow, adapt, and improve.
 
Claude (Anthropic) has been used at various points, primarily to help in the creation of documents from initial specifications. This is a hobbyist project shared openly under GNU GPL v3, to enable others who may not have in-depth technical knowledge to create their own system that matches their needs. AI LLMs can be a powerful tool to accelerate the development process. There are some graphical errors within the schematic drawings, particularly overlaps and text tightly placed. At some point these will hopefully be redrawn in a real CAD program; for now it serves its intended purpose.
 
Whilst I am comfortable in its use and recognise its value in the aforementioned acceleration, I also recognise that some find the use of AI objectionable, hence this explanation and disclosure being included. To avoid repetition it is NOT referenced in all files where Claude has been utilised, but this, or a similar disclaimer should be included if you opt to clone the repository for use in line with the GNU GPL v3.
 
---
 
## Features
 
### Current (Implemented)
 
- ✅ **48 discrete LED outputs** — model railway signals (red/green/yellow) and street lighting
- ✅ **6 turnout outputs** — LGB 18V AC point motors via SPDT relay switching
- ✅ **Block detection** — KY-024 Hall effect sensors triggered by magnets on rolling stock
- ✅ **Long cable runs** — 12V LED supply rated for 20m+ runs with minimal voltage drop
- ✅ **I²C GPIO expansion** — MCP23017 boards (×3) multiply Pi pins without dedicated wiring
- ✅ **Breadboard testing regime** — 8 stages (Stage 0–7) for incremental validation
- ✅ **Stripboard build** — permanent soldered implementation with weatherproofing
- ✅ **Browser-based control panel** — dynamic SVG track diagram, block status strip, turnout controls
- ✅ **Layout editor** — drag-and-drop track design with named save/load via server storage
- ✅ **Outdoor enclosure** — IP65 weatherproofed, DIN rail mounted, thermally managed
### Planned (Phase 9+)
 
- 🔄 **Pi API integration** — REST + WebSocket for real-time state updates
- 🔄 **Automatic signal aspect logic** — block-aware RED/YELLOW/GREEN based on occupancy
- 🔄 **BTS7960 H-bridge** — PWM traction motor control (forward/reverse/speed)
- 🔄 **ADS1115 current sensing** — traction current monitoring and fault detection
- 🔄 **Responsive UI** — tablet-friendly control panel with gestures
- 🔄 **Data logging** — session recording, playback, analytics
---
 
## Project Structure
 
```
RAIL STACK/
├── README.md                              (this file)
├── Documentation/
│   ├── Core Testing Regime/
│   │   ├── SETUP.md                       (Pi 5 initial setup)
│   │   ├── TESTING-00-INDEX.md            (testing overview)
│   │   ├── TESTING-01-STAGE-0.md          (Pi baseline)
│   │   ├── TESTING-02-STAGE-1.md          (I²C detection)
│   │   ├── TESTING-03-STAGE-2.md          (MCP output walk)
│   │   ├── TESTING-04-STAGE-3.md          (LED drivers)
│   │   ├── TESTING-05-STAGE-4.md          (relay & turnout)
│   │   ├── TESTING-06-STAGE-5.md          (Hall sensors)
│   │   ├── TESTING-07-STAGE-6.md          (integrated soak)
│   │   └── TESTING-08-STAGE-7.md          (stripboard build)
│   │
│   ├── Supplementary Guides/
│   │   ├── SOLDERING-GUIDE.md             (soldering techniques)
│   │   ├── STRIPBOARD-LAYOUT-GUIDE.md     (stripboard design)
│   │   ├── MULTIMETER-REFERENCE-GUIDE.md  (voltage/current measurement)
│   │   ├── TROUBLESHOOTING-DECISION-TREE.md (diagnostic flowcharts)
│   │   ├── COMPONENT-IDENTIFICATION-VERIFICATION.md (pre-build checks)
│   │   ├── BREADBOARD-BEST-PRACTICES.md   (layout & wiring)
│   │   ├── CABLE-TERMINATION-CONNECTOR-PREP.md (outdoor cabling)
│   │   ├── ENCLOSURE-WEATHERPROOFING.md   (IP65 assembly)
│   │   └── GPIO-PINOUT-REFERENCE-CARD.md  (printable quick-ref)
│   │
│   ├── System Documentation/
│   │   ├── SYSTEM-RECORD-SHEET.md         (fillable config form)
│   │   └── DOCUMENTATION-VERSION-TRACKER.md (version control)
│   │
│   ├── Architecture/
│   │   ├── README.md                      (hardware overview)
│   │   ├── rail-control-design-spec.md    (UI/UX specification)
│   │   └── TSANA_FORGE_BRAND_GUIDE.md     (branding standards)
│   │
├── Hardware/
│   ├── Bill of Materials
│   ├── Schematic Diagrams
│   ├── 3D Enclosure Models (CAD files)
│   └── DIN Rail Layout Diagrams
│
├── Firmware/
│   ├── python/
│   │   ├── gpio_test.py                   (Stage 0 baseline)
│   │   ├── i2c_detect.py                  (Stage 1 detection)
│   │   ├── mcp_walk.py                    (Stage 2 outputs)
│   │   ├── led_driver.py                  (Stage 3 circuits)
│   │   ├── relay_control.py               (Stage 4 turnouts)
│   │   ├── hall_sensor.py                 (Stage 5 detection)
│   │   └── rail_control.py                (main application)
│   │
│   └── libs/
│       └── (adafruit-circuitpython-mcp230xx, smbus2, etc.)
│
├── Frontend/
│   ├── rail-control.html                  (live control panel)
│   ├── rail-editor.html                   (layout design tool)
│   └── css/
│       └── rail-control.css               (Tsana Forge styled)
│
└── LICENSE                                (GNU GPL v3)
```
 
---
 
## Hardware Architecture
 
**Three completely separate power supplies** ensure safety and performance:
 
| Supply | Voltage | Use | Notes |
|---|---|---|---|
| 5V DC | 5V | Raspberry Pi, logic, relay coils | 2A minimum |
| 12V DC | 12V | LED signal outputs (48×) | 2A sufficient |
| 18V DC | 18V DC | Turnout motors (via relay) | 2A sufficient |
 
**GPIO expansion** via MCP23017 I²C boards (×3):
- **Board 0x20** — LED outputs 1–16 (signals)
- **Board 0x21** — LED outputs 17–32 (signals)
- **Board 0x22** — LED outputs 33–40 (street lights) + 6 turnout relays
**Level shifting** via TXB0104 for safe 3.3V (Pi) ↔ 5V (MCP23017) translation.
 
**Block detection** via KY-024 Hall effect sensors (up to 4 blocks monitored).
 
**Outdoor cabling** — 20m+ runs using stranded wire, screw terminals, self-amalgamating tape, and IP68 cable glands.
 
---
 
## Development Status
 
### Phase 1–6: Complete ✅
 
- Hardware architecture finalised
- Breadboard testing regime (Stages 0–7) documented and validated
- Browser-based control panel (rail-stack.html) functional
- Layout editor (rail-editor.html) with named save/load
- Outdoor enclosure design and weatherproofing procedures
### Phase 7–8: In Progress 🔄
 
- Signal aspect logic (RED/YELLOW/GREEN based on block occupancy)
- Editor QA and edge-case testing
- Responsive tablet UI polishing
### Phase 9+: Planned 📋
 
- Raspberry Pi REST API integration
- WebSocket event push (real-time updates)
- BTS7960 H-bridge traction control
- ADS1115 current sensing
- Data logging and playback
---
 
## Getting Started
 
### For Beginners
 
1. **Read SETUP.md** — Install Raspberry Pi OS, enable I²C, install Python libraries
2. **Follow the Testing Regime** — Work through Stages 0–7 in order, one stage per session
3. **Reference supplementary guides as needed**:
   - **Soldering Guide** — if you're new to soldering through-hole components
   - **Multimeter Reference** — if you're unfamiliar with voltage/current measurement
   - **Breadboard Best Practices** — for layout strategy
   - **Component Identification** — verify parts before use
4. **Fill in System Record Sheet** — document your GPIO assignments and cable routing
5. **Deploy to stripboard** — when breadboard testing passes all stages
### For Experienced Builders
 
- Skim SETUP.md and the stage overviews
- Jump directly to **Stripboard Layout Guide** for permanent build
- Reference **GPIO Pinout Reference Card** (printable) for quick lookups
- Use **Troubleshooting Decision Tree** if something fails
### For Contributing
 
See [CONTRIBUTING.md](CONTRIBUTING.md) — we welcome bug reports, documentation improvements, and feature additions.
 
---
 
## Why Documentation Matters
 
Most electronics projects share working code but assume you already know *why* it works, *how* to adapt it, and *what* to do when it breaks. **RAIL CONTROL is different.**
 
Every document in this repository was written with one goal: **to make it possible for someone with no prior experience to build and troubleshoot this system independently**.
 
- **Decision trees**, not just lists — if something fails, we show you the diagnostic process
- **Practical examples** — measurements you should see, voltages you should measure, noises you should hear
- **British and metric-first** — because good documentation respects the user's context
- **Plain language** — no jargon without explanation
- **Cross-referencing** — guides link to each other so you're never lost
This approach makes RAIL CONTROL a **blueprint for how to document hobby electronics projects** in a way that's accessible to newcomers while remaining useful to experts.
 
---
 
## Bill of Materials
 
**Approximate cost: £80–120 GBP (~$100–150 USD)** for a complete system (excluding Raspberry Pi).
 
| Item | AliExpress Search | Qty | Cost (each) |
|---|---|---|---|
| MCP23017 I²C module | `MCP23017 IIC I2C` | 3 | £2–4 |
| BC547 transistors | `BC547 transistor 100pcs` | 1 pack | £3–5 |
| 1kΩ/560Ω/680Ω resistors | `1k resistor 1/4w` | 1 pack | £1–2 |
| High-brightness 3mm LEDs | `3mm LED high brightness` | 300 pack | £5–8 |
| 8-channel relay module | `8 channel relay 5V` | 1 | £4–6 |
| KY-024 Hall sensors | `KY-024 hall effect` | 20 | £8–12 |
| TXB0104 level shifter | `4 channel logic level converter` | 1 pack | £3–5 |
| Stripboard & headers | Various | — | £5–8 |
| IP65 enclosure | `IP65 junction box 200x100` | 1–2 | £8–15 |
| Cable glands, sealant, etc. | Various | — | £10–15 |
 
**Not included:** Raspberry Pi 5 (£60–80), power supplies (£15–25), solder/tools, breadboard.
 
Full Bill of Materials in `hardware/BOM.md`.
 
---
 
## Hardware Requirements
 
- **Raspberry Pi 5** (4GB RAM minimum, 8GB recommended)
- **Micro-SD card** (32GB, class 10)
- **USB-C power supply** (5V, 2A minimum)
- **12V DC wall adapter** (2A, centre-positive)
- **18V AC transformer** (existing LGB supply, or equivalent)
- **Soldering iron** (380–420°C), solder, flux
- **Breadboard** (830-point standard or larger)
- **Multimeter** (digital, ~£5–10)
- **Hand tools** — wire stripper, crimpers, small screwdrivers
---
 
## Software & Dependencies
 
**Python 3.7+** with:
- `adafruit-circuitpython-mcp230xx` — GPIO expansion
- `smbus2` — I²C communication
- `RPi.GPIO` — Pi GPIO access
- `flask` (optional) — REST API server (Phase 9+)
**Browser requirements:**
- Modern browser (Chrome, Firefox, Edge, Safari)
- JavaScript enabled
- No external CDN dependencies (all bundled)
---
 
## Architecture & Design Decisions
 
**Why 12V for LEDs instead of 5V?**
— At 5V, a 20m cable run would experience ~2V drop, leaving LEDs too dim. 12V provides 10% headroom and allows standard resistor values (560Ω).
 
**Why MCP23017 instead of direct GPIO?**
— The Pi only has 27 usable GPIO pins. Three MCP23017 boards give us 48 outputs via just two I²C wires (SDA/SCL).
 
**Why relay switching instead of direct transistor PWM?**
— LGB EPL motors are DC devices (18V DC, rectified from the AC supply). A single SPDT relay per turnout switches polarity to throw forward/reverse; PWM would add unnecessary complexity for simple two-position switching.
 
**Why Hall effect sensors over microswitches?**
— Hall sensors are contactless (no corrosion), non-mechanical (no wear), and weather-proof. Outdoor reliability is paramount.
 
**Why stripboard instead of custom PCB?**
— For hobbyist production (1–5 units), stripboard is faster and cheaper than PCB fabrication. Single-sided layout is easier to debug and repair.
 
See `rail-stack-design-spec.md` for detailed design rationale.
 
---
 
## Known Limitations & Future Work
 
**Current limitations:**
- Signal aspect logic is manual (Phase 7 work)
- Traction PWM not yet integrated (Phase 9+)
- Current sensing not implemented (Phase 9+)
- Single-user only (no multi-client locking)
- No authentication (assumes trusted network)
**Planned improvements:**
- Responsive UI for tablets/phones
- Real-time WebSocket updates
- Block occupancy history & analytics
- Automatic train routing (pathfinding)
- Voice/app integration (via MQTT bridge)
---
 
## Contributing
 
We welcome:
- **Bug reports** — use GitHub Issues
- **Documentation improvements** — clarifications, examples, translations
- **Code contributions** — firmware enhancements, new features
- **Hardware variants** — if you've adapted this for a different scale or system
Please read [CONTRIBUTING.md](CONTRIBUTING.md) before submitting.
 
---
 
## Community & Support
 
**Questions?** Start here:
1. Check **Troubleshooting Decision Tree** in the docs
2. Search **GitHub Issues** for similar problems
3. Open a new Issue with:
   - Stage where the problem occurs
   - Symptoms (what you observed)
   - Measurements (voltages, currents if available)
   - Your setup (breadboard, stripboard, enclosure?)
**Want to share your build?** We'd love to hear about it! Post photos/videos to Discussions or link your fork on GitHub.
 
---
 
## Licence
 
**GNU GPL v3** — Free to use, modify, and distribute. Derivative works must also be GPL v3.
 
See [LICENSE](LICENSE) file for full details.
 
---
 
## About Tsana Forge
 
RAIL CONTROL is published under the **Tsana Forge** label — a collection of homelab tools, game server utilities, and hobby projects built for personal use and shared with the community.
 
Tsana Forge philosophy:
- **Honest** — tools do what they say, no marketing
- **Documented** — code means nothing without explanation
- **Accessible** — beginners should feel welcome
- **Durable** — built to last, not planned obsolescence
More at [tsana.net](https://tsana.net)
 
---
 
## Acknowledgments
 
This project was inspired by:
- JMRI (Java Model Railroad Interface) — open-source train control
- Model Railway Club enthusiasts worldwide
- Raspberry Pi community — for excellent documentation standards
Special thanks to everyone who tested early versions and provided feedback.
 
---
 
## Changelog
 
### v1.0 (April 2026)
 
- Initial release: Breadboard testing regime (Stages 0–7)
- Hardware architecture finalised
- Browser control panel and layout editor functional
- 24 comprehensive documentation files
- Outdoor enclosure design complete
- Ready for community contribution
---
 
## Contact
 
- **Issues & feature requests** — [GitHub Issues](https://github.com/tsana-forge/rail-control/issues)
- **Discussions & feedback** — [GitHub Discussions](https://github.com/tsana-forge/rail-control/discussions)
- **Email** — contact@tsana.net
---
 
*RAIL CONTROL — Raspberry Pi 5 Garden Railway Automation System*
 
*© 2026 Tsana Forge. Released under GNU GPL v3.*
 
*Part of the Tsana Forge ecosystem. [tsana.net](https://tsana.net)*
 
