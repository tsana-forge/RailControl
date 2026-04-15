# RAIL CONTROL — ENCLOSURE WEATHERPROOFING

**Version:** v1.0

> Guide to selecting, preparing, and weatherproofing an IP65/IP68 enclosure for outdoor RAIL CONTROL installation. Covers cable gland selection, silicone sealing, thermal management, drainage, and long-term environmental protection.
>
> **Last updated:** April 2026

---

## 1. INTRODUCTION

The RAIL CONTROL system lives outdoors in a garden environment. Rain, UV, thermal cycling, and corrosion are constant threats. A properly weatherproofed enclosure is the difference between a system that works for years and one that fails after the first rain.

This guide covers enclosure selection, installation, sealing, and maintenance.

---

## 2. ENCLOSURE SELECTION

### IP Rating Standards

The IP (Ingress Protection) rating has two digits:

```
IP 6 5
   │ └─ Water protection (0–9)
   └──── Dust protection (0–6)
```

| Rating | Protection | Application |
|---|---|---|
| **IP54** | Dust-protected, water splash | Indoor/covered outdoor (not recommended) |
| **IP65** | Dust-tight, low-pressure water jet | **Recommended for RAIL CONTROL** |
| **IP67** | Dust-tight, temporary immersion | Better than IP65, overkill for raised garden railway |
| **IP68** | Dust-tight, continuous immersion | Used for completely submerged equipment |

**For RAIL CONTROL (outdoor garden, not submerged):** **IP65 is ideal**. Overkill to go higher.

### Enclosure Material & Dimensions

| Material | Pros | Cons | Typical Cost |
|---|---|---|---|
| **ABS plastic** | Lightweight, UV-resistant, corrosion-proof | Can become brittle in extreme heat | £8–15 GBP |
| **Polycarbonate** | Crystal-clear, high impact strength | More expensive, not as UV-resistant | £15–25 GBP |
| **Stainless steel** | Durable, professional appearance | Expensive, conducts heat, may rust if not 316-grade | £25–50 GBP |
| **Powder-coated steel** | Strong, good thermal mass | Will rust if powder coat is damaged | £15–30 GBP |

**Recommended:** **ABS plastic IP65 enclosure**, 200 mm W × 150 mm H × 100 mm D (minimum). Larger is better (more room for components, less cramped).

### Cable Entry Points

**Plan for these connections before purchasing:**
- 1× 5V power input (USB-C or 2-pin terminal)
- 1× 12V power input (2-pin terminal)
- 1× 18V AC input (2-pin terminal)
- 4–6× Signal outputs (LED rails, relay outputs, sensor inputs)
- 1× Network (Ethernet or WiFi antenna)

**Total: 7–9 cable glands minimum** (buy an enclosure with room for this many).

---

## 3. CABLE GLAND SELECTION & INSTALLATION

### Cable Gland Types

| Type | Thread | Range | Cost | Best For |
|---|---|---|---|---|
| **PG7** | M20 | 3.5–6 mm | £1–2 each | Signal wires, sensor cables |
| **PG11** | M25 | 5–10 mm | £1–2 each | Power wires, bundled cables |
| **PG13.5** | M32 | 6–13 mm | £2–3 each | Thick bundles, multiple wires |
| **M20 IP67** | M20 | 6–10 mm | £3–4 each | Professional-grade, better sealing |

**For RAIL CONTROL:** Buy a mixed pack: 5× PG7 (signals), 3× PG11 (power), 1× spare.

### Cable Gland Installation Procedure

**Step 1: Drill the hole**

```
Enclosure front panel
        │
    ┌───┼───┐
    │   O   │  ← Hole diameter = gland thread size
    │   │   │
    └───┴───┘
```

| Gland | Hole Size (Metric) | Hole Size (Imperial) |
|---|---|---|
| PG7 | 20 mm | 25/32 inch |
| PG11 | 25 mm | 1 inch |
| PG13.5 | 32 mm | 1.25 inch |

**Tool:** Hole saw or step drill bit (cleaner than spade bit).

**Step 2: Insert the gland**

```
        Outside
            │
      ┌─────┼─────┐
      │           │
   [·O·]  ← Gland (threaded into hole)
      │           │
      └─────┼─────┘
            │
        Inside
```

- Insert the **lock ring** (external part) into the hole from outside
- Thread the **gland body** (internal part) from inside, turning clockwise
- Tighten until hand-tight + ¼ turn (do not over-tighten or plastic will crack)

**Step 3: Insert cable and seal**

- Push the cable through the open gland
- The **rubber insert** (strain relief) squeezes around the cable as you tighten the inner nut
- Tighten the inner nut (same hand-tight + ¼ turn)
- Cable should not slide out if you tug it firmly

**Step 4: Test seal**

```
Spray water from a hose around the gland:
- Water should not enter the enclosure
- If water drips inside, the gland is not tight
  → Tighten the inner nut 1/8 turn more
```

---

## 4. INTERIOR ENCLOSURE LAYOUT

### DIN Rail Installation

Standard DIN rail (35 mm wide, 7.5 mm tall) holds breakers, terminal blocks, and circuit boards.

**Installation:**

1. **Position rails** — Mount two parallel 35 mm DIN rails, 100 mm apart (horizontally)
2. **Mark mounting holes** — Pencil mark where the rail clips will sit
3. **Drill holes** — 3 mm diameter (for M3 self-tapping screws or standoff studs)
4. **Screw rails in place** — Use M3 screws or adhesive-backed standoffs
5. **Snap on components** — Breakers, terminal blocks, and DIN-rail PCBs clip on

```
        Enclosure interior (top view)

    ┌──────────────────────────────┐
    │ Cable gland entry (top-left)  │
    │        ││                     │
    │  [Terminal block]             │  ← 5V/12V/GND distribution
    │        ││                     │
    │  [DIN rail, rail 1]           │
    │  ┌──────────────────────┐     │
    │  │ [MCP23017 PCB] [Relay│     │
    │  │                  Module]   │
    │  └──────────────────────┘     │
    │        ││                     │
    │  [DIN rail, rail 2]           │
    │  ┌──────────────────────┐     │
    │  │ [Fan/ventilation]    │     │
    │  └──────────────────────┘     │
    │        ││                     │
    │  [Drainage hole (bottom)]     │
    │                               │
    └──────────────────────────────┘
```

### Component Arrangement

**Zone 1 (top-left) — Power Entry:**
- 5V input terminal block
- 12V input terminal block
- Common GND terminal block
- 2A fuse holders (5V and 12V rails)

**Zone 2 (top-right) — Signal Processing:**
- MCP23017 boards (mounted on DIN clips or adhesive standoffs)
- Level shifter (mounted on breadboard or small PCB)
- Pull-up resistors (if not on MCP boards)

**Zone 3 (bottom-left) — Outputs:**
- 8-channel relay module (DIN-mounted or screwed to enclosure)
- Terminal blocks for turnout motor connections
- Terminal blocks for sensor returns

**Zone 4 (bottom-right) — Ventilation:**
- Small DC fan (12V, if thermal management needed — see Section 6)
- Inlet air vents (covered with mesh to exclude insects)

---

## 5. SEALING & DRAINAGE

### Primary Sealing: Silicone Sealant

After all components are installed and wired, seal internal gaps and cable entry points.

**Materials:**
- Clear or white silicone sealant (food-grade is best, non-toxic)
- Caulk gun
- Wet finger (for smoothing)

**Sealing procedure:**

1. **Cable entry sealing**
   ```
   Around each cable gland, inside the enclosure:
   ┌────────────────┐
   │ Enclosure wall │
   │      │         │ ← Cable
   │    [○]         │
   │     Silicone   │ ← Bead of sealant, fills gaps
   │      ~~~~~     │
   │      Interior  │
   ```
   - Apply a ¼-inch bead of silicone around the cable exit
   - Smooth with a wet finger
   - Cure per sealant instructions (24 hours typical)

2. **Terminal block sealing**
   - Seal around the top and sides of each terminal block
   - Don't seal the wire entry points (needs to remain accessible)

3. **Enclosure seam sealing** (optional, for extra protection)
   - If the enclosure has a visible seam, apply silicone along the inside
   - This prevents water from entering through the seam over time

### Drainage Design

**Water will eventually enter the enclosure** (during heavy rain or temperature fluctuations). Plan for this:

1. **Drainage hole** (1× 5 mm diameter)
   - Drill at the **lowest corner** of the enclosure
   - Insert a small PVC grommet or rubber washer to prevent sharp edges
   - Water drains out; moisture evaporates

2. **Cable routing**
   - Route cables so that any water drains toward the drainage hole
   - Avoid trapping water in cable coils inside the enclosure

3. **Desiccant (optional)**
   - Place a small silica gel packet inside the enclosure
   - Replace every 6 months or when saturated
   - Absorbs moisture and prevents corrosion

### Moisture Prevention

In high-humidity environments (coastal gardens, frequent rain):

1. **Reduce air circulation** — Seal most cable entry points after final installation
2. **Ventilation** — Use a small exhaust fan (12V, draws air out and moisture away)
3. **Thermal control** — Keep enclosure in shade if possible (prevents condensation from temperature swings)

---

## 6. THERMAL MANAGEMENT

### Heat Generation in RAIL CONTROL

The main heat sources:
- **Relay coils** — ~1 W per relay (minimal)
- **Transistor drivers** — ~0.5 W each (48 transistors = 24 W worst-case, but not all on simultaneously)
- **Raspberry Pi** — ~5 W (more in summer)
- **MCP23017 chips** — <0.5 W total

**Total worst-case:** ~30 W

In an outdoor enclosure with sunlight, interior temperature can exceed 60°C (140°F), causing:
- Electrolytic capacitors to dry out (shortened lifespan)
- Transistor gain to decrease (LEDs may dim)
- Relay contacts to oxidise faster

### Passive Cooling

**For most garden installations, passive cooling is sufficient:**

1. **Enclosure colour** — Use white or light grey (reflects heat, not black)
2. **Orientation** — Face cable glands downward (prevents water entry, allows air circulation)
3. **Shading** — Install the enclosure in afternoon shade if possible
4. **Ventilation holes** — Drill 2× small (5 mm) holes at **top of enclosure** (hot air rises out)
5. Cover vent holes with **fine mesh** (excludes insects, allows airflow)

**Expected interior temperature:** ~5–10°C above ambient (usually acceptable).

### Active Cooling (If Needed)

For high-current installations or very hot climates:

1. **12V DC fan** — Small computer fan (30–40 mm, 12V, 0.1 A)
2. **Installation:**
   - Mount on DIN rail with thermal epoxy
   - Connect to 12V rail (with inline 1 A fuse)
   - Route exhaust toward the top of the enclosure
   - Install intake vent at bottom (air flows up and out)

3. **Thermostat control (optional):**
   - Use a simple **thermostat relay module** (available on AliExpress)
   - Fan turns on when interior exceeds 50°C
   - Turns off when cools below 40°C
   - Prevents constant fan operation (saves power)

---

## 7. LONG-TERM WEATHERPROOFING MAINTENANCE

### 6-Month Inspection Checklist

- [ ] Check cable glands for looseness (water entry) — retighten if needed
- [ ] Inspect silicone seals for cracks or separation — re-seal if damaged
- [ ] Check for rust or corrosion on metal parts — apply touch-up paint if needed
- [ ] Verify drainage hole is clear (not clogged with dirt or leaves)
- [ ] Replace desiccant packet if it appears saturated
- [ ] Check interior for moisture (open enclosure on a dry day, look for water droplets)

### Annual Maintenance

- [ ] Deep clean: Open enclosure, remove components, wipe down with a dry cloth
- [ ] Inspect all solder joints for cold joints or corrosion (use magnifying glass)
- [ ] Re-apply silicone sealant to any cracks or peeling areas
- [ ] Verify cable glands are still tight (outdoor vibration can loosen them)
- [ ] Check component leads for corrosion (green or white oxidation on metal)

### Addressing Water Damage

**If water is found inside the enclosure:**

1. **Turn off power immediately** — Risk of short circuit
2. **Remove and dry components**
   - Take out PCBs, relay modules, terminal blocks
   - Dry with a soft, lint-free cloth
   - If heavily corroded, use isopropyl alcohol on a cloth (evaporates quickly)
   - Let dry for 24 hours in a warm, dry place

3. **Inspect damage**
   - Check for corrosion (green/white deposits on copper)
   - Check solder joints (may have cracked from thermal stress)
   - Check component leads (may be corroded)

4. **Repair**
   - Clean corroded areas with a fine brush or contact cleaner
   - Re-solder any cold joints (see Soldering Guide)
   - Replace any heavily corroded components

5. **Identify the water source**
   - Check cable glands (are they loose?)
   - Check silicone seals (are they cracked?)
   - Check drainage hole (is it clogged?)
   - Reseal before closing enclosure

---

## 8. OUTDOOR INSTALLATION CHECKLIST

Before deploying the enclosure:

- [ ] Enclosure is IP65 or better
- [ ] All cable glands are installed and tested for water ingress
- [ ] Interior is sealed with silicone (cured for 24 hours)
- [ ] Drainage hole is clear and grommetted
- [ ] DIN rail is secure and level
- [ ] All components are mounted and secured (no loose items inside)
- [ ] Power distribution is star-grounded (see Cable Termination guide)
- [ ] All wiring is bundled and labelled
- [ ] Cooling strategy is in place (passive or active)
- [ ] Enclosure is positioned for shade (if high ambient temperature expected)
- [ ] Cable runs to track are routed safely (not a trip hazard)
- [ ] Initial power-up test passes (all supplies, I²C bus, GPIO)

---

## 9. ENVIRONMENTAL CONSIDERATIONS

### UV Degradation

**Problem:** Sunlight degrades plastic and rubber over time.

**Solutions:**
- Mount enclosure in shade (under a pergola, against a fence)
- Use UV-resistant paint or clear UV-protective coating on exposed surfaces
- Replace desiccant packets annually (UV degrades silica gel)

### Temperature Cycling

**Problem:** Temperature swings cause condensation inside the enclosure.

**Solutions:**
- Ensure drainage hole is clear (allows moisture to escape)
- Use desiccant packets (absorb moisture)
- Ventilation holes (top and bottom) promote air circulation
- Avoid excessive thermal mass (black enclosure in full sun will heat too much)

### Salt Corrosion (Coastal Installations)

**Problem:** Salt air corrodes metal connectors and solder joints.

**Solutions:**
- Use stainless steel hardware (if not already)
- Apply a thin coat of clear lacquer to exposed copper leads
- Use dielectric grease on terminal connections (repels moisture)
- Increase inspection frequency (every 3 months instead of 6)

### Insect Intrusion

**Problem:** Spiders, insects, and debris enter through cable glands and vents.

**Solutions:**
- Cover all ventilation holes with fine mesh (1 mm holes)
- Reduce cable gland gaps with tight sealing (no large gaps)
- Avoid leaving doors/openings unsealed during downtime
- Inspect for webs and debris during maintenance

---

## DOCUMENTATION REFERENCES

When weatherproofing the enclosure, refer to:

- **Cable Termination & Connector Prep** — For cable entry point planning
- **Troubleshooting Decision Tree** — If water damage causes electrical faults
- **Component Identification & Verification** — For component specifications and thermal limits

---

## LICENCE

This guide is released under the **GNU GPL v3**. You are free to use, modify, and distribute. All derivative works must also be released under GNU GPL v3.

For full details, see the `LICENCE` file or visit https://www.gnu.org/licenses/gpl-3.0.html

---

*Tsana Forge — RAIL CONTROL | Enclosure Weatherproofing | v1.0 | https://tsana.net*
