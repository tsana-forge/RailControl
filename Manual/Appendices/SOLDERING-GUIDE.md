# RAIL CONTROL — SOLDERING GUIDE

**Version:** v1.0

> Complete reference for soldering through-hole components, from tool selection through joint inspection. Covers the specific soldering tasks required for the G Scale Garden Railway control system.
>
> **Version:** Build 10 | Last updated: April 2026

---

## 1. OVERVIEW

Soldering is the process of joining electrical components using a metal alloy (solder) melted by heat. This guide covers **through-hole soldering** — the technique for soldering component leads through plated holes in PCB or stripboard.

You will perform through-hole soldering in this project for:

- **Stage 7 final build:** Mounting MCP23017 IC sockets, BC547 transistors, resistors, and terminal blocks on stripboard
- **Enclosure wiring:** Connecting screw terminals and DIN rail blocks to the stripboard

---

## 2. TOOLS & MATERIALS

### 2.1 Essential Tools

| Tool | Purpose | Recommendation |
|---|---|---|
| **Soldering iron** | Heat source | 40–60W, temperature-controlled preferred |
| **Solder** | Joining alloy | Lead-free, 60/40 or SAC305 alloy, 0.8–1.0mm diameter |
| **Sponge or brass wire cleaner** | Iron tip cleaning | Damp natural sponge or brass coil (brass is better) |
| **Helping hands / PCB holder** | Stabilise work | Alligator clip arms or magnetic PCB holder |
| **Side cutters** | Trim component leads | Sharp, able to cut copper wire cleanly |
| **Wire strippers** | Prepare wire ends | Adjustable, 20–28 AWG range |
| **Multimeter** | Verify joints | Continuity mode for inspection |

### 2.2 Materials

| Material | Purpose | Notes |
|---|---|---|
| **Solder (lead-free)** | Joint material | 60/40 (tin/copper) or SAC305 (tin/silver/copper). Lead-free has higher melting point (~220°C vs 183°C). |
| **Flux** | Joint wetting agent | Rosin core (built into solder) or liquid flux in a pen. Flux cleans oxides and improves solder flow. |
| **Desoldering wick** | Remove excess solder | Copper braid that absorbs molten solder. Essential for fixing mistakes. |
| **Solder sucker** | Remove excess solder | Spring-loaded vacuum device (alternative to wick). |
| **Wet sponge or damp paper** | Clean iron tip | Keep tip clean between joints. |

### 2.3 Setup Checklist

- [ ] Soldering iron plugged in and heating (allow 2–3 minutes to reach temperature)
- [ ] Sponge/brass cleaner wetted and placed near the iron
- [ ] Solder, wick, and solder sucker within arm's reach
- [ ] Side cutters and wire strippers on the workspace
- [ ] Component packs sorted and organised by value
- [ ] Stripboard or PCB mounted in helping hands
- [ ] Multimeter nearby for testing

---

## 3. SOLDERING IRON SETUP

### 3.1 Temperature Selection

**Recommended:** 380–420°C (716–788°F for reference) for lead-free solder

Lead-free solder requires higher heat than lead-based solder. Too cold, and the solder won't flow properly (cold joint). Too hot, and you risk damaging components or the PCB.

> ⚠️ **Do not exceed 450°C.** Above this temperature, PCB traces can lift, component legs can burn, and the solder itself can degrade.

### 3.2 Tip Selection & Maintenance

**Tip styles:**
- **Chisel tip** (flat, angled) — best for through-hole work on stripboard. Transfers heat evenly.
- **Conical tip** (pointed) — good for fine work, but less heat transfer.
- **Bevel tip** — specialty for high-density work (not needed here).

**Keep the tip clean:**

- Wipe the tip on the wet sponge or brass cleaner **before and after every joint**
- A clean tip transfers heat much more efficiently
- Oxidised tips (dull, crusty appearance) conduct heat poorly — clean them regularly
- If the tip is heavily corroded, replace it (tips are inexpensive)

---

## 4. SOLDER PROPERTIES & SELECTION

### 4.1 Solder Composition

**Lead-free (recommended for this project):**
- **SAC305** — 96.5% tin, 3.0% silver, 0.5% copper. Industry standard. Melting point ~217°C.
- **60/40** — 60% tin, 40% copper. Older standard (requires lead-free equivalent). Melting point ~190°C.

**Lead-based (older standard, no longer recommended):**
- **60/40** — 60% tin, 40% lead. Melting point ~190°C. Easier to solder but toxic. Avoid for new projects.

For this project, use **lead-free solder, 0.8–1.0mm diameter,** with **rosin core flux.**

### 4.2 Flux

Flux is a chemical that removes oxides from metal surfaces, allowing solder to wet the joint properly.

**Types:**
- **Rosin core** — built into solder wire. Sufficient for most work. Leaves a brown, slightly sticky residue (harmless).
- **No-clean flux** — minimal residue. Good for high-reliability work.
- **Liquid flux (pen)** — applied separately before soldering. Use if rosin core isn't flowing well.

You will only need rosin core flux (already in your solder). Liquid flux is optional for difficult joints.

---

## 5. THROUGH-HOLE SOLDERING TECHNIQUE

### 5.1 The Four-Step Process

#### Step 1: Place the Component

- Insert the component lead through the PCB hole
- If the lead is too thick, pre-tin it (see Section 5.5)
- Bend the lead slightly on the back side to hold it in place (or use helping hands to stabilise)

#### Step 2: Heat the Joint

- Touch the iron tip to **both the lead and the PCB pad simultaneously**
- Hold for 2–3 seconds to heat both surfaces
- Do **not** apply solder to the iron tip — apply it to the joint itself

> ⚠️ **Critical:** If you heat only the solder, you get a cold joint. Always heat the lead and pad.

#### Step 3: Apply Solder

- Once the lead and pad are hot, touch the solder wire to the joint (not the iron)
- Solder should flow smoothly and wet the joint
- Use a small amount — typically a 5–10mm length of solder wire per joint
- Solder should form a smooth, shiny cone around the lead

#### Step 4: Remove the Iron and Solder

- Pull both the iron and solder away simultaneously
- Let the joint cool for 2–3 seconds before moving or touching it
- **Do not blow on the joint** — let it cool naturally (fast cooling can create weak joints)

**Timing:** The entire process should take 3–5 seconds per joint.

---

### 5.2 Visual Inspection

**Good joint:**
- Shiny, smooth cone of solder
- Solder completely fills the area between the lead and pad
- No excess solder (should not blob out or spread beyond the pad)
- Lead is completely wetted (solder adheres to the lead, not just the pad)

**Bad joint (cold joint):**
- Dull, grainy, or blobby appearance
- Solder forms a ball rather than a cone
- Lead not fully wetted — solder sits on top like a drop of water on wax

**Excess solder (solder bridge):**
- Too much solder connecting adjacent pads or traces
- Can cause short circuits

**Insufficient solder (dry joint):**
- Visible gaps between lead and solder
- Joint is mechanically weak

### 5.3 Flux Residue

After soldering, you will see a brownish, slightly tacky residue around the joint. This is rosin flux and is harmless — it does not need to be cleaned off for hobby projects. If you prefer a clean appearance, clean it off with:

- Isopropyl alcohol + soft brush (preferred)
- Desoldering wick (absorbs the residue)
- Leave it (perfectly fine for this project)

---

## 5.4 Component-Specific Techniques

### Resistors & Capacitors (Passive Components)

1. Insert both leads through adjacent holes
2. Bend leads on the back side to hold in place
3. Trim excess lead (about 1–2mm remaining)
4. Solder both leads with the same technique as above
5. Trim remaining lead flush with solder joint (use side cutters)

### Transistors (TO-92 Package)

The BC547 has three leads arranged in a line. Leads are thinner than resistors and bend easily.

1. Insert all three leads through the holes
2. Hold gently with helping hands (do not bend excessively)
3. Solder each lead individually (2–3 seconds per lead)
4. Trim excess lead after soldering
5. Double-check pinout (E–B–C, left to right) against the schematic

### IC Sockets (28-pin DIP)

Use IC sockets rather than soldering ICs directly (allows chip replacement if it fails).

1. Insert the socket with the notch aligned to the schematic marking
2. Solder only the **four corner pins** first (this holds the socket in place)
3. Inspect that the socket is straight and aligned
4. Solder the remaining pins (all 28)
5. Use a wet sponge to clean the tip between every 4–5 pins (residue buildup reduces heat transfer)

### Terminal Blocks & Headers

These have thicker pins and require more heat.

1. Insert the terminal block and ensure it is seated flat against the PCB
2. Apply more solder than usual — these joints carry higher currents
3. Solder all pins with the same technique, but hold the iron slightly longer (3–4 seconds) due to the larger thermal mass
4. Inspect for full wetting (solder should completely surround the pin)

---

## 5.5 Pre-Tinning (Optional but Helpful)

Pre-tinning is the process of coating a lead with a thin layer of solder before inserting it into the PCB. This is optional but helpful for difficult leads or when soldering is slow.

**To pre-tin a lead:**

1. Heat the lead with the soldering iron
2. Touch solder wire to the hot lead
3. A thin layer of solder adheres to the lead
4. Remove the solder and iron
5. Insert the pre-tinned lead into the PCB and solder normally (requires less solder and heat)

---

## 6. FIXING MISTAKES

### 6.1 Cold Joint (Dull, Grainy Appearance)

**Fix:**
1. Reheat the joint with the soldering iron for 3–4 seconds
2. Add a tiny bit of fresh solder (a few mm of wire)
3. Remove the iron and let cool

The fresh solder provides flux that cleans the joint and improves wetting.

### 6.2 Excess Solder (Blob or Bridge)

**Fix:**
1. Place desoldering wick on top of the excess solder
2. Heat the wick + joint with the soldering iron for 2–3 seconds
3. The wick absorbs the molten solder
4. Remove the wick and iron

Alternatively, use a solder sucker (vacuum device) to remove excess solder.

### 6.3 Solder Bridge (Connecting Two Adjacent Pads)

**Fix:**
1. Use desoldering wick (preferred) — place it across both pads and heat until solder is absorbed
2. Or use the side of a clean iron tip to drag excess solder away from the bridge
3. Inspect with a multimeter in continuity mode — should be no beep between the bridged traces

### 6.4 Insufficient Solder (Dry Joint)

**Fix:**
1. Add more solder by reheating the joint and touching solder wire to it
2. Let the fresh solder flow into the gap
3. The existing solder + new solder should merge into a smooth cone

### 6.5 Damaged Component or Pad

If a component leg breaks or a pad lifts off the PCB during soldering:

1. **Broken leg on a passive component (resistor/capacitor):** Replace the component with a new one
2. **Broken pin on an IC:** This is why you use IC sockets — replace the socket, not the chip
3. **Lifted pad:** This is a PCB fault. If it happens on stripboard, carefully solder a wire directly to the adjacent trace as a workaround

---

## 7. SOLDERING SAFETY

### 7.1 Personal Safety

- **Always solder in a well-ventilated area** — open a window or use a fume extractor
- **Wear safety glasses** — solder can splatter
- **Keep water nearby** — for treating minor burns (soldering irons are 350°C+, so treat burns seriously)
- **Never touch the tip** — always assume it is hot
- **Unplug the iron when finished** — do not leave it on unattended

### 7.2 PCB/Component Safety

- **Avoid overheating components** — if a joint takes more than 5 seconds, let it cool and try again
- **Do not solder near plastic components** — plastic IC sockets can melt if the iron is too close
- **Check component polarity before soldering** — electrolytic capacitors and diodes only work one way
- **Keep the iron tip clean** — oxidised tips conduct heat poorly and can leave cold joints

---

## 8. SOLDERING CHECKLIST — STAGE 7 BUILD

Use this checklist when you begin soldering the stripboard for Stage 7.

### Pre-Soldering

- [ ] Soldering iron plugged in and heating to 380–420°C
- [ ] Tip is clean and shiny (wiped on wet sponge)
- [ ] Solder, wick, sucker within reach
- [ ] Stripboard mounted in helping hands
- [ ] Components sorted by value
- [ ] Schematic or layout diagram at hand
- [ ] Multimeter nearby for testing

### Soldering

- [ ] Solder IC socket corner pins first, inspect alignment
- [ ] Solder remaining IC socket pins (clean tip every 4–5 pins)
- [ ] Solder all resistors (trim excess lead after)
- [ ] Solder all transistors (BC547s) — verify E–B–C pinout
- [ ] Solder terminal blocks (allow extra heat, verify full wetting)
- [ ] Inspect each joint visually (shiny cone, no bridges, no dry joints)

### Post-Soldering

- [ ] Allow all joints to cool naturally (do not blow on them)
- [ ] Run multimeter continuity test: no shorts between 5V and GND, no shorts between 12V and GND
- [ ] Inspect stripboard under magnification (look for solder bridges between adjacent tracks)
- [ ] Remove desoldering wick residue if present (optional — rosin flux is harmless)
- [ ] Unplug soldering iron and allow to cool

---

## 9. TROUBLESHOOTING

| Symptom | Cause | Action |
|---|---|---|
| Solder won't flow or wet the joint | Iron too cold, or joint not clean | Increase iron temperature slightly (try 400°C). Ensure both lead and pad are heated simultaneously. |
| Solder forms a ball instead of a cone | Lead or pad not hot enough | Hold the iron on the joint longer (3–4 seconds) before applying solder. |
| Component lead breaks when inserting into PCB | Lead is too rigid or PCB is brittle | Pre-bend the lead gently before inserting. If PCB hole is tight, use a 1.0mm drill bit to enlarge it slightly. |
| Solder bridge between adjacent pads | Too much solder applied, or iron held too long | Use desoldering wick to remove excess solder. Reheat the bridge and drag the wick across it. |
| Joint looks dull and grainy | Cold joint — solder cooled before fully flowing | Reheat the joint and add fresh solder (a few mm of wire). The flux in fresh solder cleans the joint. |
| Ic socket pins not wetting | Oxidised or corroded pins | Clean the socket legs with a brass brush before soldering. Pre-tin each pin if the problem persists. |
| Uneven heating (one side of socket hot, other side cold) | Iron tip not making full contact | Rotate the iron so the flat side of the chisel tip makes contact with the pin and pad simultaneously. |

---

## 10. ADVANCED TECHNIQUES (OPTIONAL)

### 10.1 Soldering in Tight Spaces

If components are close together and the iron tip doesn't fit:

1. Use a narrower iron tip (conical instead of chisel)
2. Pre-tin the component lead before inserting it
3. Use liquid flux to improve solder flow
4. Work quickly to avoid heating adjacent components

### 10.2 Desoldering an Entire Component

To remove a component you soldered incorrectly:

1. **Desoldering wick method:** Place wick over all pins, heat with iron, wick absorbs solder
2. **Solder sucker method:** Heat joint, trigger the sucker to vacuum out molten solder
3. **Combination:** Heat the joint, use wick to absorb most solder, then carefully pull the component out with tweezers

For IC sockets, you may need to desolder all 28 pins — this takes patience. Work methodically from one end to the other.

### 10.3 Rework & Repair

If you must remove and resolder an old joint:

1. Heat the existing solder until it melts (2–3 seconds)
2. Use wick or sucker to remove as much solder as possible
3. Add fresh solder (the flux cleans the joint)
4. Resolder the component

The existing solder may have lost its flux, so fresh solder provides new flux and improves wetting.

---

## 11. REFERENCE

### Solder Temperature Guide

| Alloy | Melting Point | Recommended Iron Temp | Use Case |
|---|---|---|---|
| 60/40 Pb (lead-based) | 188°C | 350–380°C | Old standard, phased out |
| SAC305 (lead-free) | 217°C | 380–420°C | Modern standard, recommended |
| 63/37 Pb (lead-based) | 183°C | 350–380°C | Eutectic, easier to solder |

### Flux Types

| Type | Residue | Cleaning | Use Case |
|---|---|---|---|
| Rosin core | Brown, sticky | Optional | Standard for hobby work |
| No-clean | Minimal | Optional | High-reliability applications |
| Liquid (separate) | Minimal | Optional | For difficult joints |

### Component Lead Gauges

| Component | Lead Diameter | Notes |
|---|---|---|
| Resistor/capacitor | 0.6–0.8mm | Easy to solder |
| Transistor (TO-92) | 0.5–0.6mm | Thin, handle gently |
| IC socket pin | 0.7–0.8mm | Standard, requires more heat |
| Terminal block | 1.0–1.5mm | Thick, requires extended heating |

---

## LICENCE

This guide is released under the **GNU GPL v3**. You are free to use, modify, and distribute. All derivative works must also be released under GNU GPL v3.

For full details, see the `LICENCE` file or visit https://www.gnu.org/licenses/gpl-3.0.html

---

*Tsana Forge — RAIL CONTROL | Soldering Guide | v1.0 | https://tsana.net*
