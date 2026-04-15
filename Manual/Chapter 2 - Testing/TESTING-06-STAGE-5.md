# RAIL CONTROL — STAGE 5 — HALL EFFECT SENSORS

**Version:** v1.0

> Verify KY-024 sensors detect a magnet at the expected distance and produce clean digital output suitable for block detection.
>
> **Version:** Build 10 | Last updated: April 2026

---

## 1. GOAL

Confirm sensors trigger reliably at the mounting distance required by G scale track geometry. Adjust sensitivity potentiometers. Test edge-detection interrupt handling for clean, bounce-free triggers.

---

## 2. EQUIPMENT REQUIRED

- [ ] Raspberry Pi 5 (Stage 0 complete — no MCP23017 needed)
- [ ] KY-024 Hall effect sensor modules × 3 minimum
- [ ] Neodymium magnets (the ones for rolling stock)
- [ ] Ruler or callipers
- [ ] Small screwdriver (potentiometer adjustment)

---

## 3. WIRING

```
KY-024 VCC ──► Pi 3.3V (PIN 1)
KY-024 GND ──► Pi GND  (PIN 6)
KY-024 DO  ──► Pi GPIO 4
KY-024 AO  ──► not connected
```

For multi-sensor tests, add sensors on GPIO 5 and GPIO 6 using same VCC/GND.

---

## 4. TESTS

### Test 4.1 — Static Detection

```python
import RPi.GPIO as GPIO
import time

GPIO.setmode(GPIO.BCM)
GPIO.setup(4, GPIO.IN, pull_up_down=GPIO.PUD_UP)

while True:
    state = GPIO.input(4)
    print(f"GPIO 4: {'CLEAR' if state else 'MAGNET DETECTED'}")
    time.sleep(0.2)
```

- [ ] Output reads `CLEAR` with no magnet
- [ ] Output reads `MAGNET DETECTED` when magnet held near sensor
- [ ] Output returns to `CLEAR` when magnet removed

---

### Test 4.2 — Trigger Distance Measurement

Place sensor flat on bench. Slowly lower magnet toward it from above. Record distance at which DO goes LOW.

| Attempt | Distance | Notes |
|---|---|---|
| 1 | ___ mm | |
| 2 | ___ mm | |
| 3 | ___ mm | |
| **Average** | **___ mm** | |

- [ ] Average trigger distance recorded
- [ ] Distance sufficient for your track mounting gap

---

### Test 4.3 — Potentiometer Sensitivity Adjustment

The blue trimmer on the KY-024 sets the digital trigger threshold.

- Clockwise = more sensitive (triggers from further away)
- Anti-clockwise = less sensitive

- [ ] Adjusted so magnet triggers reliably at expected distance + 5mm margin
- [ ] Confirmed: sensor does NOT false-trigger without magnet
- [ ] Re-measured trigger distance after adjustment: ___ mm

---

### Test 4.4 — Pass-By Simulation

Move the magnet past the sensor at realistic speed (walking pace).

- [ ] DO output goes LOW briefly and returns HIGH
- [ ] Pulse duration observed: approximately ___ ms (expect 200–500ms at walking speed)
- [ ] Trigger consistent across 10 passes

---

### Test 4.5 — Edge Detection (Interrupt-Driven)

```python
import RPi.GPIO as GPIO
import time

GPIO.setmode(GPIO.BCM)
GPIO.setup(4, GPIO.IN, pull_up_down=GPIO.PUD_UP)

def sensor_callback(channel):
    print(f"TRIGGERED on GPIO {channel} at {time.time():.3f}")

GPIO.add_event_detect(4, GPIO.FALLING, callback=sensor_callback, bouncetime=200)
print("Waiting for magnet... (Ctrl+C to exit)")

try:
    while True:
        time.sleep(1)
except KeyboardInterrupt:
    GPIO.cleanup()
```

- [ ] Exactly one callback per magnet pass
- [ ] No double-triggers

Test different bouncetimes:

| Bouncetime (ms) | Double triggers? | Pass |
|---|---|---|
| 200 | [ ] yes / [ ] no | [ ] |
| 300 | [ ] yes / [ ] no | [ ] |
| 500 | [ ] yes / [ ] no | [ ] |

- [ ] Optimal bouncetime identified: ___ ms

---

### Test 4.6 — Multiple Sensors

Wire 2–3 sensors on GPIO 4, 5, 6. Run edge detection on all simultaneously.

- [ ] Each sensor triggers independently
- [ ] Triggering one does not cause false triggers on others
- [ ] All three sensors reliable with the same bouncetime

---

## 5. PASS CRITERIA

All of the following must be true before proceeding to **Stage 6**:

- [ ] Sensors detect magnet at sufficient distance for track mounting
- [ ] Potentiometers adjusted — no false triggers, reliable detection
- [ ] Clean single-trigger per pass with edge detection
- [ ] Multiple sensors operate independently without interference
- [ ] Optimal bouncetime identified and recorded

---

## 6. MEASURED VALUES

| Item | Value |
|---|---|
| Trigger distance (pot adjusted) | ___ mm |
| Mounting gap (rail + baseboard) | ___ mm |
| Margin (trigger − gap) | ___ mm |
| Optimal bouncetime | ___ ms |
| Pass-by pulse duration | ___ ms |

---

*Tsana Forge — RAIL CONTROL | Hardware Testing Regime Stage 5 | v1.0 | https://tsana.net*
