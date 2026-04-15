# RAIL CONTROL — CABLE TERMINATION & CONNECTOR PREP

**Version:** v1.0

> Guide to preparing and terminating outdoor cables for the RAIL CONTROL system. Covers wire stripping, screw terminal installation, connector assembly, strain relief, and weatherproofing techniques for long cable runs (20+ metres) in garden environments.
>
> **Last updated:** April 2026

---

## 1. INTRODUCTION

RAIL CONTROL sends signals and power over long outdoor cables (5–30 metres from the enclosure to track). Cable termination is critical — a loose terminal or corroded connector will cause intermittent failures and frustrating troubleshooting.

This guide walks through proper termination, weatherproofing, and connector assembly techniques.

---

## 2. WIRE SELECTION FOR OUTDOOR RUNS

### Wire Gauge by Application

| Application | Gauge | Colour | Max Length | Current |
|---|---|---|---|---|
| 5V signal (GPIO, sensor returns) | 22 AWG | Green, yellow | <20 m | 100 mA |
| 12V LED supply | 20 AWG | Red | <30 m | 2 A |
| 18V AC (relay switching) | 18 AWG | Orange | <30 m | 5 A |
| 5V power distribution | 20 AWG | Red | <20 m | 2 A |
| GND returns | 20 AWG | Black | <20 m | 2 A |

### Stranded vs Solid Core

| Type | Use | Pros | Cons |
|---|---|---|---|
| **Stranded** | Outdoor, flexible runs | Withstands flexing, vibration; won't crack with thermal cycling | Thicker, harder to crimp accurately |
| **Solid core** | Breadboard, fixed enclosure | Thinner, easier to route, easier to crimp | Brittle in cold; cracks with repeated bending |

**For RAIL CONTROL:** Use **stranded wire exclusively** for outdoor runs. Solid core will work-harden and snap in the first winter.

### Voltage Drop Over Distance

For long cable runs, voltage drop is significant:

```
Voltage drop = (2 × Length × Current) / (Wire gauge constant)

Example: 12V supply, 20 AWG stranded, 15 metre run, 1 A load
Drop = (2 × 15 × 1) / 10.45 ≈ 2.9 V
Arrival voltage = 12V − 2.9V = 9.1V ← LED will be dim
```

**Solution:** Use one gauge thicker (e.g. 18 AWG instead of 20 AWG) for runs >15 metres.

---

## 3. WIRE STRIPPING & TINNING

### Stripping Stranded Wire (The Right Way)

**Tool:** Wire stripper (adjustable, 18–22 AWG range)

**Procedure:**

1. Adjust stripper to correct gauge (22 AWG slot for 22 AWG wire)
2. Insert wire 12 mm (0.5 inch) into the stripper
3. **Rotate the stripper 2–3 times around the wire** (don't pull yet)
4. Gently pull the stripper toward the wire end
5. Twist the loose insulation and pull it off the conductor

**What NOT to do:**
- ❌ Don't use scissors or a knife (crushes strands, creates sharp edges)
- ❌ Don't strip >12 mm of insulation (exposed conductor will corrode outdoors)
- ❌ Don't strip <6 mm (not enough to grip in terminal)

### Exposed Conductor Length

**Correct:**
```
 ┌─ Insulation (12 mm minimum, 15 mm typical)
 │
[─────────────────┐
                  └─ Bare conductor (6–8 mm)
                      to insert in terminal
```

**Incorrect — too short:**
```
[─────┐
      └─ Only 3 mm bare ← Terminal won't grip properly
```

**Incorrect — too long:**
```
[──────────────────────┐
                       └─ 20 mm bare ← Will corrode, create short
```

### Tinning Stranded Wire (Optional but Recommended)

Tinning (coating with a thin layer of solder) prevents strands from fraying and improves terminal grip.

**Procedure:**

1. Strip the wire as above
2. Heat the bare conductor with a soldering iron (380–420°C)
3. Apply a small amount of solder (lead-free if possible)
4. Let the solder flow around all the strands
5. Cool for 5 seconds before touching
6. Result: a stiff, compact conductor that won't fray

**Benefits:**
- Strands won't separate when inserted in terminal
- Better electrical contact in screw terminal
- Less likely to corrode (solder seals exposed copper)

---

## 4. SCREW TERMINAL INSTALLATION

### Terminal Types Used in RAIL CONTROL

| Type | Pins | Current | Use |
|---|---|---|---|
| Phoenix contact 5 mm pitch | 2 or 3 | 10 A | Power supply entry, signal returns |
| Wire-to-board terminal | 2 | 5 A | LED/relay outputs |
| Barrier strip | 2–8 | 20 A | Heavy-duty power distribution |

### Installing a Wire in a Screw Terminal

**Step 1: Prepare the wire**

```
Stripped wire with 6–8 mm bare conductor
Optional: tinned with solder for compactness
```

**Step 2: Open the terminal**

- For push-cage terminals: press the small lever/button to open the contact
- For screw terminals: turn the screw counter-clockwise 3–4 turns (don't remove it)

**Step 3: Insert the wire**

```
        Screw (lever up)
           ↓
    ┌─────────────┐
    │             │ ← Contact opens
 ───┤ Wire →      │
    │             │
    └─────────────┘
```

- Push the bare conductor straight in until it bottoms out (≥5 mm insertion depth)

**Step 4: Tighten the screw**

- Turn the screw clockwise firmly (3–5 N of torque — thumb tightness)
- **Do NOT over-tighten** (2–3 full rotations from open position)
- Wire should not pull out with moderate tugging force

**Step 5: Verify connection**

```
Multimeter in continuity mode:
- Touch one probe to the wire insulation (should show continuity through terminal)
- Touch the other probe to the terminal block metal
- Beep = connection is good
- No beep = wire not inserted fully or screw is loose
```

### Common Termination Mistakes

| Mistake | Result | Fix |
|---|---|---|
| Wire inserted <3 mm | Loose contact, high resistance | Remove, reinsert fully, tighten |
| Screw over-tightened | Insulation crushed, short circuit risk | Back off 1/4 turn |
| Screw under-tightened | Intermittent connection, resistance fluctuates | Tighten 1/4 turn |
| Tinning prevents full insertion | Wire bottoms out 2 mm before contact | Use thinner solder layer, or don't tin |
| Multiple strands separated | Some strands not in contact | Re-strip, tin if using stranded |

---

## 5. STRAIN RELIEF & MECHANICAL PROTECTION

### Strain Relief at Enclosure Entry

Long cables experience tension and vibration. Without strain relief, the connection will eventually fail.

**Strain relief design:**

```
        Enclosure wall
             │
   ┌─────────┼─────────┐
   │         │         │  ← Cable gland (IP68, prevents water entry)
   │      [····]       │
   │        │          │
   │    Cable loop ←┐  │
   │    (loose coil)│  │  ← 50 mm minimum radius bend
   │        │       │  │
   │      [····]  ──┘  │
   │        │          │
   │   Screw terminal   │
   │        │          │
   └────────┼──────────┘
            │
        To track
```

**Key points:**
- Cable should enter the enclosure **at a shallow angle** (not sharp 90°)
- Inside the enclosure, **coil the cable with 50 mm radius minimum** (don't kink)
- Use a **cable clip or Velcro strap** to hold the coil in place
- Leave **150 mm of slack** before the screw terminal (allows future retermination)

### Mechanical Protection Outdoors

**Cable routing:**
- Keep cables away from foot traffic
- Route along fence posts or through cable conduit if exposed
- Bury underground cables at least **300 mm deep** in PVC conduit

**Cable ties & clips:**
- Use UV-resistant nylon cable ties (not steel clips, which rust)
- Tighten ties firmly but not excessively (hand-tight is sufficient)
- Re-check ties every 6 months for slackness (outdoor vibration causes loosening)

---

## 6. WEATHERPROOFING CONNECTIONS

### Self-Amalgamating Tape (Primary Sealing Method)

Self-amalgamating (self-fusing) tape is the best weatherproofing solution for outdoor connectors.

**How it works:**
- The tape is made of rubber that bonds to itself (not adhesive)
- Wrapping creates an airtight, watertight seal
- UV-resistant and lasts 5+ years

**Application procedure:**

1. **Prepare the connection**
   - Ensure wires are clean and dry
   - Screw terminal is tightened
   - No debris or dust on the terminal

2. **Wrap the tape**
   ```
   Starting from the insulation, overlap each wrap by 50%:
   
   Wire insulation
        │
        ├─ [Tape wrap 1]
        ├─ [Tape wrap 2] ← Overlaps by 50%
        ├─ [Tape wrap 3] ← Overlaps by 50%
        │
   Screw terminal
   (visible, not covered)
   ```
   - Start at the wire insulation, 25 mm from the terminal
   - Wrap around the connection 3–5 times
   - Overlap each wrap by 50% of the tape width
   - End on the terminal block side

3. **Press firmly**
   - Smooth the tape with your fingers
   - Press down at each wrap to ensure good contact
   - The tape will bond within 30 minutes

### Silicone Sealant (Secondary Waterproofing)

For critical connections or extra protection, apply clear silicone after the tape.

**Procedure:**
1. Allow self-amalgamating tape to cure (30 minutes minimum)
2. Apply a bead of clear silicone around the screw terminal and wrapped tape
3. Smooth with a wet finger (reduces tackiness)
4. Cure per silicone instructions (typically 24 hours)

**Advantage:** Creates a secondary moisture barrier and UV shield.

---

## 7. CONNECTOR ASSEMBLY FOR MODULAR CABLES

For frequently disconnected cables (e.g. turnout motor leads), use weatherproof connectors instead of permanent screw terminals.

### Weatherproof Connector Types

| Type | Pins | Current | Application |
|---|---|---|---|
| IP67 M12 (aviation connector) | 4–8 | 10 A | Premium; overkill for RAIL CONTROL |
| Phoenix Contact SACC-M (mini circular) | 3–5 | 10 A | Professional; expensive |
| Anderson PowerPole (Anderson SB50) | 50 A | 2 circuit | Common in car electronics; bulky |
| Sealed Dupont (micro-connectors) | 2–4 | 3 A | Budget; prone to corrosion unless sealed |

**Recommended for RAIL CONTROL:** Sealed **Dupont-style connectors** or simple **screw terminals with self-amalgamating tape** (cheaper, less complex).

### Hand-Crimping Dupont Connectors for Outdoor Use

**Tools needed:**
- Dupont crimp tool (or small jeweller's pliers)
- Dupont connectors (two-piece: contact + housing)
- Wire stripper (22 AWG for signal, 20 AWG for power)
- Self-amalgamating tape (for sealing)

**Procedure:**

1. **Strip wire** — 6 mm bare conductor (see Section 3)

2. **Insert wire into connector**
   ```
   Connector housing:  │ ← Wire goes here
                       │
   Contact (folded):   └─┐
                         │ ← Wraps around conductor
   ```
   - Slide the wire into the contact
   - The contact should grip the bare conductor and insulation

3. **Crimp with pliers**
   - Use the **insulation crimp slot** (larger slot) on the contact if available
   - Press firmly 2–3 times
   - Wire should not pull out with moderate tugging

4. **Insert contact into housing**
   - Push the contact fully into the plastic housing
   - Hear a small "click" (if the housing has a retention clip)

5. **Seal with tape**
   - Wrap self-amalgamating tape around the connector housing
   - 3–4 wraps, overlapping by 50%
   - This waterproofs the joint

**Testing:**
- Multimeter continuity: should beep when connected
- Pull test: connector should not separate with 5 N of force
- Water test: hold connector under running water for 10 seconds, then test continuity (should still beep)

---

## 8. EXTERNAL CABLE RUN CHECKLIST

Before deploying a cable outdoors:

- [ ] Wire gauge is correct (18–20 AWG for power, 22 AWG for signals)
- [ ] Wire is stranded (not solid core)
- [ ] Insulation is clean and dry
- [ ] Both ends are stripped and tinned (optional but recommended)
- [ ] Both ends are terminated (screw terminal or crimp connector)
- [ ] Screw terminals are tight (pull test passes)
- [ ] Connections are sealed with self-amalgamating tape
- [ ] Cable has strain relief (50 mm radius minimum) at enclosure
- [ ] Cable is routed away from foot traffic
- [ ] Cable is clipped or secured every 1–2 metres
- [ ] Multimeter continuity test passes
- [ ] Voltage drop is within acceptable range (test under load if possible)

---

## 9. TROUBLESHOOTING CABLE CONNECTIONS

### Intermittent Connection (Works, Then Stops)

**Likely causes:**
1. Screw terminal is loose
2. Strands are separated inside the terminal
3. Corrosion on the terminal or wire

**Quick test:**
```
Multimeter in continuity mode:
- Test the connection (should beep)
- Wiggle the cable gently (beep should persist)
- If beep disappears, connection is loose
```

**Fix:**
1. Turn off power
2. Remove the wire from the terminal
3. Check the bare conductor (should be smooth, not oxidised)
4. Re-insert fully and tighten the screw
5. Retest

### High Resistance (Multimeter shows >10Ω in terminal)

**Likely causes:**
1. Partial insertion (only partial conductor in contact)
2. Corrosion on the wire or terminal
3. Screw terminal is damaged

**Fix:**
1. Remove the wire
2. Clean the bare conductor with a dry cloth or fine sandpaper
3. Clean the inside of the terminal with a dry cloth
4. Re-insert the wire fully
5. Tighten the screw firmly
6. Test resistance again (should be <1Ω)

### Short Circuit Between Wires

**Symptom:** Multimeter shows continuity between two wires that should be isolated (e.g. 5V and GND)

**Likely causes:**
1. Self-amalgamating tape has failed
2. Water has entered the terminal
3. Wire insulation is damaged

**Fix:**
1. Turn off power immediately
2. Dry the connection with a clean cloth
3. Remove the old tape
4. Inspect the wire insulation (should have no cracks)
5. If insulation is damaged, cut the connection and re-terminate with new wire
6. Re-apply self-amalgamating tape
7. Test continuity again (should show no beep between isolated circuits)

---

## DOCUMENTATION REFERENCES

When terminating cables, refer to:

- **Troubleshooting Decision Tree** — If termination is suspect in a fault
- **Soldering Guide** — For tinning wire before termination
- **Component Identification & Verification** — For terminal block specifications

---

## LICENCE

This guide is released under the **GNU GPL v3**. You are free to use, modify, and distribute. All derivative works must also be released under GNU GPL v3.

For full details, see the `LICENCE` file or visit https://www.gnu.org/licenses/gpl-3.0.html

---

*Tsana Forge — RAIL CONTROL | Cable Termination & Connector Prep | v1.0 | https://tsana.net*
