# RAIL CONTROL — STRIPBOARD LAYOUT PLANNING GUIDE

**Version:** v1.0

> Complete reference for designing and executing stripboard layouts for the G Scale Garden Railway control system. Covers track cutting, component placement, routing, and verification before soldering.
>
> **Version:** Build 10 | Last updated: April 2026

---

## 1. OVERVIEW

Stripboard (also called Veroboard) is a PCB with copper tracks running horizontally along one side and plated-through holes on a 2.54 mm (0.1 inch) grid. You solder components to one side; the copper tracks on the other side create electrical connections.

This guide covers designing a layout for the LED driver circuits, MCP23017 sockets, and terminal blocks required in **Stage 7** of the testing regime.

---

## 2. STRIPBOARD BASICS

### 2.1 Physical Properties

Standard stripboard for this project:

| Property | Value |
|---|---|
| Dimensions | 9 cm × 15 cm (35 × 59 holes) |
| Hole pitch | 2.54 mm (0.1 inch) |
| Copper track orientation | Horizontal (left to right) |
| Hole diameter | 0.8–1.0mm (suitable for component leads) |
| Pad diameter | ~2mm (solder joint area) |
| Substrate | Phenolic (fibreglass alternative is more expensive but better) |

### 2.2 Track Layout

Stripboard has rows of copper tracks running horizontally. Each row is electrically continuous from left to right **unless intentionally broken** by drilling out the copper between holes.

```
Row 1:  ●――●――●――●――●――●――●  (continuous copper track)
Row 2:  ●――●――●――●――●――●――●  (continuous copper track)
Row 3:  ●――●――●――●――●――●――●  (continuous copper track)
```

To break a track (isolate two adjacent holes), you drill out the copper between them using a 3mm bit or hobby knife.

---

## 3. DESIGN PRINCIPLES

### 3.1 Component Placement Strategy

**Vertical arrangement (recommended for this project):**

Place components in columns running top to bottom. This keeps wiring short and organised.

```
      Col 1   Col 2   Col 3
Row 1: [R1]    [R2]    [R3]
Row 2: [Q1]    [Q2]    [Q3]
Row 3: [LED]   [LED]   [LED]
```

**Horizontal arrangement:**

Components arranged left to right along a row. Useful for linear circuits (voltage regulators, filter chains). Less ideal for parallel circuits (multiple identical drivers).

### 3.2 Minimise Wire Routing

- Keep component leads short — solder them directly to the stripboard when possible
- Route wires along the edges of the stripboard (less visual clutter, easier to trace)
- Use different coloured wires for different signals (12V, GND, I²C SDA/SCL, etc.)

### 3.3 Track Cutting Strategy

**Cut tracks to:**
- Isolate different power domains (separate 5V and 12V sections)
- Break the continuous track between component pins that need to be at different potentials
- Create a logical separation between circuit sections

**Don't cut tracks unnecessarily** — each cut is a potential weak point if not done cleanly.

---

## 4. DESIGNING A LAYOUT FOR THIS PROJECT

### 4.1 Circuit Sections

The stripboard build for this project consists of three main sections:

| Section | Components | Purpose |
|---|---|---|
| **I²C Interface** | TXB0104, MCP23017 sockets ×3, pull-up resistors | Level shifting and GPIO expansion |
| **LED Driver Bank** | BC547 transistors ×6–16, 1kΩ base resistors, 560Ω/680Ω LED current limiters | Drive red/yellow/green/white LEDs |
| **Relay Control** | Relay module interface, fuse holder, terminal blocks | Switch 18V AC turnout motors |

### 4.2 Power Distribution

Plan separate power rails for 5V and 12V:

```
     5V Rail (top or dedicated track)
     ├── Pi VCC
     ├── MCP23017 VCC
     └── TXB0104 HV power

     12V Rail (separate track or external)
     ├── LED cathode (common rail)
     └── External 12V supply

     GND Rail (common to all)
     ├── Pi GND
     ├── MCP23017 GND
     ├── TXB0104 GND
     └── All signal returns
```

**Recommendation:** Dedicate one horizontal track for 5V, one for 12V, and one (or two) for GND. This keeps power distribution simple and allows you to verify no shorts during testing.

### 4.3 Layout Example (Single Stripboard with 6 LED Drivers)

This is a minimal example for **testing purposes**. You will scale this to the full 48 drivers on three stripboards.

```
     Col:  1    2    3    4    5    6    7    8    9    10
Row 1:    [R1]  -   [R2]  -   [R3]  -   GND  -    -    -
Row 2:    [Q1] [Q2] [Q3] [Q4] [Q5] [Q6]  -   GND  -    -
Row 3:    [560][560][560][560][560][560] |   |    -    -
Row 4:     |    |    |    |    |    |   [GND BUS] -    -
Row 5:    [LED][LED][LED][LED][LED][LED]  |   |    -    -
Row 6:     |    |    |    |    |    |  12V+  |    -    -

Legend:
[R1] = 1kΩ base resistor
[Q1] = BC547 transistor
[560] = 560Ω LED current limiting resistor
[LED] = 3mm LED (anode side, cathode to 12V rail)
GND  = Common ground
```

**Key features:**
- Row 1–2: Base resistors (1kΩ) and transistor bases
- Row 3: LED current limiting resistors
- Rows 4–5: LED anodes
- Row 6: 12V+ rail for LED cathodes
- Vertical cuts between columns to isolate each driver stage

---

## 5. STEP-BY-STEP LAYOUT DESIGN

### Step 5.1 — Sketch on Paper First

Before touching the stripboard, draw a scaled layout on graph paper (1 square = 1 hole).

**Create a checklist:**

- [ ] Total number of components (transistors, resistors, LEDs, IC sockets, terminal blocks)
- [ ] Power requirements (how many 5V, 12V, GND connections needed)
- [ ] Input connections (I²C SDA/SCL from Pi, signal lines from MCP23017)
- [ ] Output connections (screw terminals for external cables, relay connections)
- [ ] Available space on stripboard (35 holes wide × 59 holes tall)

**Example inventory for one driver board:**

| Component | Qty | Purpose |
|---|---|---|
| BC547 transistor | 16 | Drive 16 LEDs |
| 1kΩ resistor | 16 | Base current limiting |
| 560Ω resistor | 12 | Red/yellow/white LED current limit |
| 680Ω resistor | 4 | Green LED current limit |
| LED (mixed colours) | 16 | Visible outputs |
| Connecting wire | ~50cm | Point-to-point routing |

### Step 5.2 — Identify Track Cuts

On your paper sketch, mark where copper tracks need to be broken.

**Rules for identifying cuts:**

- Between two holes that must be at different potentials (e.g. left side 5V, right side GND)
- Between transistor collector (output) and the next stage (isolate each driver)
- Between base resistor and LED current limiting resistor (different signal levels)

**Mark cuts with an X on your sketch:**

```
Col:  1    2    3    4
Row:  ●――X――●    ●――X――●
      |      |    |      |
      (cut between cols 2–3)
```

### Step 5.3 — Plan External Wiring Points

Mark where wires will connect to:

- **Inputs:** I²C SDA/SCL from Pi (via TXB0104), signal lines from MCP23017
- **Outputs:** LED anodes (or connect directly to stripboard), 12V supply connection
- **Power:** 5V in, 12V in, GND returns

**Use designated holes at the board edges** for external connections (easier to trace and less prone to accidental shorts).

### Step 5.4 — Assign Holes to Each Component

On your paper sketch, assign specific row/column pairs to each component.

**Naming convention:**

- Row numbers: 1–59 (top to bottom)
- Column letters: A–AJ (left to right) — or just use numbers 1–35

**Example:**
- Q1 (transistor): R10 C1 C2 (rows 10, columns 1–2; transistor pins occupy 3 holes)
- R1 (1kΩ): R8 C3 C4
- LED1: R12 C1 to R14 C1 (LED anode at R12 C1, cathode at R14 C1)

This prevents errors during the actual soldering.

---

## 6. TRACK CUTTING PROCEDURE

### 6.1 Tools Needed

- [ ] Stripboard
- [ ] 3mm drill bit
- [ ] Cordless drill or hand drill
- [ ] Ruler or callipers (measure hole positions)
- [ ] Marker pen (optional, mark cut locations)
- [ ] Multimeter (verify cuts afterwards)

### 6.2 Cutting Technique

**Using a 3mm drill bit (recommended):**

1. Place the stripboard on a soft surface (foam, wood — not metal)
2. Align the 3mm drill bit over the copper track between the two holes you want to isolate
3. Gently twist the bit by hand (do **not** use a power drill — too aggressive) or use a cordless drill on **low speed**
4. Twist until the copper between the holes is removed (you should see a small copper disc separate)
5. **Do not drill into the holes themselves** — you only want to remove the copper bridge between them

**Using a hobby knife (alternative):**

1. Score the copper track between the holes with several passes of a sharp hobby knife
2. Angle the knife to cut along the track (perpendicular to the holes)
3. Eventually the copper will separate

### 6.3 Verification After Cutting

Use a multimeter in **continuity mode:**

1. Set multimeter to continuity (beep mode)
2. Touch one probe to a hole on the left side of the cut
3. Touch the other probe to a hole on the right side of the cut
4. **Should NOT beep** (indicates the track is successfully broken)
5. If it beeps, the copper is still connected — repeat the cutting process

---

## 7. COMPONENT PLACEMENT STRATEGY

### 7.1 Placement Order (Suggested)

Solder components in this order to keep the board manageable:

1. **IC sockets** (if using them) — solder all corner pins first, verify alignment, then solder remaining pins
2. **Resistors** (all values) — start with 1kΩ, then 560Ω/680Ω, then any pull-up resistors
3. **Transistors** (BC547s) — verify pinout (E–B–C) before soldering
4. **Connecting wires** (point-to-point) — after all components are mounted
5. **Terminal blocks** (last) — after all soldering is complete

This order keeps the board organised and allows you to verify each section before moving to the next.

### 7.2 Orientation & Alignment

**Resistors and capacitors:**
- Orientation doesn't matter electrically (non-polarised)
- Align them horizontally or vertically for neatness

**Transistors (BC547 TO-92 package):**
- Must be oriented correctly: **E–B–C** (left to right, flat face toward you)
- Use your schematic or a marking pen to label the position before soldering

**IC sockets (28-pin DIP):**
- Notch or dot indicates pin 1 position
- Align the notch to match the schematic marking
- This ensures the IC chip (inserted later) is oriented correctly

**LEDs:**
- **Polarity matters:** long leg (anode) goes to the signal side, short leg (cathode) goes to GND/12V+
- Mark the board with + and − before soldering to avoid errors

---

## 8. WIRING CONVENTIONS

### 8.1 Colour Coding

Use different wire colours for different signal types. This makes debugging much easier.

| Signal | Colour | Example |
|---|---|---|
| 5V power | Red | Pi VCC → TXB0104 → MCP23017 |
| 12V power | Yellow or Orange | 12V PSU → LED cathode rail |
| Ground | Black or Brown | Pi GND → common bus |
| I²C SDA | Green | Pi SDA → TXB0104 → MCP23017 |
| I²C SCL | Blue | Pi SCL → TXB0104 → MCP23017 |
| Signal (MCP output) | White or Grey | MCP23017 → transistor base |

### 8.2 Wire Gauge

Use **22 AWG stranded wire** for external connections (off-board) and **20 AWG solid core** for internal routing on the stripboard.

> Solid core is easier to route on stripboard; stranded is more flexible for external cables.

### 8.3 Point-to-Point Wiring

On stripboard, you will use short jumpers between component leads. Recommended approach:

1. **Use component leads themselves** where possible (resistor lead to transistor lead)
2. **Use bare copper wire** for short jumps (<2 cm) between tracks
3. **Use insulated wire** for longer jumps or when crossing other wires (to avoid accidental shorts)

**Keep wire lengths minimal:**
- Short wires = less resistance = better signal integrity
- Also improves heat dissipation from transistors
- Looks neater

---

## 9. VERIFICATION BEFORE SOLDERING

### 9.1 Pre-Soldering Checklist

Before you start soldering, verify everything on your layout plan:

- [ ] All track cuts are marked on the stripboard (use a marker pen)
- [ ] All component positions are identified (use your sketch)
- [ ] IC socket orientation is correct (notch aligned)
- [ ] Transistor orientation is correct (E–B–C, left to right)
- [ ] LED polarities are marked (anode/cathode)
- [ ] Power rails (5V, 12V, GND) are clearly identified
- [ ] External wire connection points are marked
- [ ] No conflicts (two components assigned to the same hole)

### 9.2 Dry-Fit Assembly (Optional but Recommended)

Before soldering, insert all components into the stripboard and verify:

1. All leads fit cleanly through the holes
2. No mechanical interference between adjacent components
3. IC sockets are straight and aligned
4. No components overhang the board edge

If something doesn't fit, adjust your layout and re-plan before soldering.

---

## 10. SOLDERING EXECUTION

### 10.1 Soldering Order (Re-Check Section 7.1)

Solder in the order listed:

1. IC sockets (corner pins first)
2. All resistors
3. All transistors (verify pinout)
4. Connecting wires
5. Terminal blocks

### 10.2 Track Cuts (Do This First)

**Before soldering any components:**

1. Perform all track cuts as planned
2. Verify each cut with a multimeter in continuity mode
3. Do not proceed to soldering until all cuts are confirmed

### 10.3 Soldering Each Component

Refer to the **Soldering Guide** (Section 5) for detailed technique. Key points:

- Heat both the lead and the pad simultaneously (2–3 seconds)
- Apply solder to the joint (not the iron)
- Solder should form a smooth cone, not a blob
- Trim excess lead after soldering (use side cutters)
- Clean the iron tip on a wet sponge between every 4–5 joints

### 10.4 Quality Control During Soldering

After soldering each section (e.g. all resistors, then all transistors):

- [ ] Visual inspection under magnification (look for cold joints, bridges, insufficient solder)
- [ ] Multimeter continuity test (verify no shorts between adjacent tracks)
- [ ] Allow all joints to cool naturally (do not blow on them)

---

## 11. POST-SOLDERING VERIFICATION

### 11.1 Visual Inspection

Under magnification (or with a smartphone camera zoomed in):

- [ ] All joints are shiny and smooth (cone-shaped)
- [ ] No dull, grainy, or blobby joints (cold joints)
- [ ] No solder bridges between adjacent pads or tracks
- [ ] All component leads are fully wetted by solder
- [ ] No stray solder blobs on the board
- [ ] All track cuts are clean and complete

### 11.2 Electrical Tests

**Continuity test (multimeter in continuity mode):**

- [ ] 5V rail has continuity from left to right (no breaks)
- [ ] 12V rail has continuity from left to right
- [ ] GND rail has continuity across the board
- [ ] No short between 5V and GND
- [ ] No short between 12V and GND
- [ ] No short between 5V and 12V

**Voltage test (multimeter in DC voltage mode):**

- [ ] 5V PSU connected: measure 5V between 5V rail and GND (should read ~5.0V)
- [ ] 12V PSU connected: measure 12V between 12V rail and GND (should read ~12.0V)
- [ ] Do NOT apply signal inputs yet — just verify power rails

### 11.3 Component-Specific Tests

**IC sockets:**
- [ ] Insert a known-good MCP23017 (or another 28-pin DIP chip)
- [ ] Verify the chip is fully seated and level
- [ ] Remove the chip (do not power on yet)

**Transistors:**
- [ ] Visually verify pinout (E–B–C, left to right, on all BC547s)
- [ ] Check that base resistor is connected to the base pin
- [ ] Check that collector is isolated from the next stage (via track cut)

**LEDs:**
- [ ] Verify anode (long leg) is on the signal side
- [ ] Verify cathode (short leg) is connected to 12V+ or GND as planned
- [ ] Do not apply power yet — just visual check

---

## 12. SCALING TO FULL PRODUCTION (48 LED Drivers)

The testing regime requires three stripboards (one per MCP23017 board):

| Board | Drivers | Resistors | Layout |
|---|---|---|---|
| Board 1 | 16 LED drivers | 16 × 1kΩ, 16 × 560Ω | Columns 1–8 |
| Board 2 | 16 LED drivers | 16 × 1kΩ, 16 × 560Ω | Columns 1–8 |
| Board 3 | 8 LED drivers + relay interface | 8 × 1kΩ, 8 × 560Ω, relay circuit | Columns 1–6 |

**Recommended scaling:**

1. Build Board 1 with **6 drivers as a test** (use Section 4.3 layout)
2. Verify all 6 drivers work (Stage 3 LED test)
3. Scale up to 16 drivers on Board 1 (same layout, more columns)
4. Repeat for Board 2 and Board 3

This reduces risk — if something goes wrong on Board 1, you haven't wasted time on all three boards.

---

## 13. TROUBLESHOOTING LAYOUT ISSUES

| Problem | Cause | Solution |
|---|---|---|
| Component lead doesn't fit the hole | Hole too small or lead too thick | Drill the hole to 1.0mm if needed, or pre-tin the lead and insert carefully |
| Two components are mechanically touching | Layout too dense | Adjust your layout to space components further apart |
| Solder bridge between adjacent pads | Tracks not cut, or cut incompletely | Verify the cut is complete with a multimeter. Recut if necessary. |
| Track cut in the wrong place | Planning error | Use desoldering wick to remove components near the cut, carefully re-drill, and re-solder |
| Multimeter shows short between 5V and GND | Solder bridge or incomplete track cut | Trace the short visually. Use desoldering wick to remove excess solder. Verify track cuts. |
| IC socket pins are corroded or won't solder | Oxidised copper from age or humidity | Clean the socket legs with a brass brush. Pre-tin each pin. Apply fresh flux. |

---

## 14. DESIGN REFERENCE TEMPLATE

Copy this template for planning your own stripboard layouts:

```
Stripboard Layout — [Project Name]
Date: ___________
Stripboard size: 9cm × 15cm (35 holes wide, 59 holes tall)

COMPONENT INVENTORY
Component type | Qty | Notes
_______________|_____|_______________

TRACK CUTS REQUIRED
Location (col-col, row) | Purpose
___________________|___________

EXTERNAL CONNECTIONS
Signal | Wire colour | Stripboard position
_______|_____________|_________________

ASSEMBLY ORDER
1. IC sockets (corners first)
2. All resistors
3. All transistors
4. Connecting wires
5. Terminal blocks

VERIFICATION CHECKLIST
[ ] Track cuts complete and verified with multimeter
[ ] All components placed and aligned
[ ] Visual inspection (no solder bridges, cold joints)
[ ] Continuity test (5V rail, 12V rail, GND rail)
[ ] No shorts between power rails
[ ] Voltage test (5V reads 5.0V, 12V reads 12.0V)
```

---

## LICENCE

This guide is released under the **GNU GPL v3**. You are free to use, modify, and distribute. All derivative works must also be released under GNU GPL v3.

For full details, see the `LICENCE` file or visit https://www.gnu.org/licenses/gpl-3.0.html

---

*Tsana Forge — RAIL CONTROL | Stripboard Layout Planning Guide | v1.0 | https://tsana.net*
