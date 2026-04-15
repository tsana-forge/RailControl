# RAIL CONTROL — STAGE 4 — RELAY MODULE & TURNOUT MOTOR

**Version:** v1.0

> Verify the 8-channel relay module switches correctly under MCP23017 control and can throw an LGB turnout motor via 18V AC.
>
> **Version:** Build 10 | Last updated: April 2026

---

## 1. GOAL

Confirm the relay module's active-LOW logic, verify contact switching with a multimeter, then connect a real LGB turnout motor and tune the pulse duration for reliable throws.

---

## 2. EQUIPMENT REQUIRED

- [ ] Raspberry Pi 5 + MCP23017 #3 at address 0x22 (Stages 0–2 complete)
- [ ] 8-channel relay module (5V, optoisolated)
- [ ] LGB transformer (18V AC) — existing layout supply
- [ ] LGB turnout motor × 1
- [ ] 2A fuse + PCB fuse holder
- [ ] Multimeter (continuity + AC voltage)
- [ ] Jumper wires

---

## 3. WIRING

```
MCP23017 #3 GPB0 (pin 8) ──► Relay module IN1
Pi 5V                      ──► Relay module VCC
Pi GND                     ──► Relay module GND

18V AC Live ──► 2A Fuse ──► Relay 1 COM
Relay 1 NO  ──► LGB Motor Terminal 1
Relay 1 NC  ──► LGB Motor Terminal 2
LGB Motor other terminal ──► 18V AC Neutral
```

> ⚠️ **Do not connect the motor until Tests 4.1–4.3 pass.** Verify relay operation dry first.

---

## 4. TESTS

### Test 4.1 — Relay Click Test (No Motor)

```python
import board, busio, time
from adafruit_mcp230xx.mcp23017 import MCP23017
from digitalio import Direction

i2c = busio.I2C(board.SCL, board.SDA)
mcp3 = MCP23017(i2c, address=0x22)

relay_pin = mcp3.get_pin(8)  # GPB0 = pin 8
relay_pin.direction = Direction.OUTPUT
relay_pin.value = True   # relay OFF (active LOW)

# Energise
relay_pin.value = False  # relay ON
time.sleep(0.5)
relay_pin.value = True   # relay OFF
```

- [ ] Relay clicks audibly when GPIO goes LOW
- [ ] Relay clicks back when GPIO goes HIGH
- [ ] Relay module indicator LED lights when energised

---

### Test 4.2 — Contact Verification

Set multimeter to continuity mode.

**Relay de-energised (GPIO HIGH):**
- [ ] COM–NC: continuity (beep)
- [ ] COM–NO: open circuit

**Relay energised (GPIO LOW):**
- [ ] COM–NO: continuity (beep)
- [ ] COM–NC: open circuit

---

### Test 4.3 — Confirmed Active-LOW Logic

- [ ] Confirmed: GPIO LOW = relay energised (relay LED on, click heard)
- [ ] Confirmed: GPIO HIGH = relay de-energised (relay LED off)

> This is critical — software must account for inverted logic.

---

### Test 4.4 — Turnout Motor Pulse Test

Connect the LGB motor and 18V AC supply per the wiring diagram. **Keep fingers clear of the turnout mechanism.**

```python
def throw_turnout(pin_num, mcp, pulse_ms=300):
    pin = mcp.get_pin(pin_num)
    pin.direction = Direction.OUTPUT
    pin.value = False          # energise relay
    time.sleep(pulse_ms / 1000)
    pin.value = True           # release relay

throw_turnout(8, mcp3)
```

- [ ] Turnout blades move fully in one direction when relay pulsed
- [ ] Blades remain in the other position when relay released
- [ ] Motor sound is a brief snap, not continuous buzz

> If it buzzes continuously, the relay is being held too long — confirm the script releases after the pulse.

---

### Test 4.5 — Pulse Duration Tuning

Test different pulse lengths to find the minimum reliable duration.

| Pulse (ms) | Throws fully? | Notes |
|---|---|---|
| 200 | [ ] yes / [ ] no | |
| 300 | [ ] yes / [ ] no | |
| 400 | [ ] yes / [ ] no | |

- [ ] Minimum reliable pulse identified: ___ ms

---

### Test 4.6 — Rapid Toggle Test

Throw the turnout 20 times with 2-second gaps.

- [ ] All 20 throws completed
- [ ] No misfires (turnout failed to throw fully)
- [ ] No relay sticking
- [ ] Fuse intact

---

## 5. PASS CRITERIA

All of the following must be true before proceeding to **Stage 5**:

- [ ] Relay toggles cleanly with active-LOW logic
- [ ] Multimeter confirms correct COM/NO/NC contact behaviour
- [ ] Turnout throws fully in both directions
- [ ] Minimum pulse duration identified and recorded
- [ ] 20 rapid throws complete without misfire or sticking

---

## 6. MEASURED VALUES

| Item | Value |
|---|---|
| Minimum reliable pulse | ___ ms |
| 18V AC measured voltage (no load) | ___ V AC |
| Relay module VCC measured | ___ V |
| Motor current during throw | ___ mA |
| Fuse rating confirmed | 2A |

---

*Tsana Forge — RAIL CONTROL | Hardware Testing Regime Stage 4 | v1.0 | https://tsana.net*
