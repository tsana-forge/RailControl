# RAIL CONTROL — STAGE 6 — INTEGRATED BENCH TEST

**Version:** v1.0

> Run all subsystems simultaneously on the bench. Test for I²C stability, power supply headroom, and noise immunity before permanent wiring.
>
> **Version:** Build 10 | Last updated: April 2026

---

## 1. GOAL

This is the prove-it-all-works-together stage. Every component from Stages 1–5 runs at the same time. Key risks: I²C errors under load, relay switching noise causing false sensor triggers, power supply droop under full load.

---

## 2. EQUIPMENT REQUIRED

- [ ] Everything from Stages 1–5, assembled on breadboard
- [ ] Multimeter (DC current mode)
- [ ] 100nF ceramic capacitors × 3 (standby, for noise fix if needed)
- [ ] Laptop/terminal for monitoring script output

---

## 3. TESTS

### Test 3.1 — Full I²C Bus Under Load

All three MCP boards connected. Walk all 48 LED outputs while reading hall sensors.

```python
import board, busio, time
import RPi.GPIO as GPIO
from adafruit_mcp230xx.mcp23017 import MCP23017
from digitalio import Direction

i2c = busio.I2C(board.SCL, board.SDA)
boards = [
    MCP23017(i2c, address=0x20),
    MCP23017(i2c, address=0x21),
    MCP23017(i2c, address=0x22),
]

GPIO.setmode(GPIO.BCM)
for sp in [4, 5, 6]:
    GPIO.setup(sp, GPIO.IN, pull_up_down=GPIO.PUD_UP)

errors = 0
for mcp in boards:
    for p in range(16):
        try:
            pin = mcp.get_pin(p)
            pin.direction = Direction.OUTPUT
            pin.value = True
            time.sleep(0.1)
            pin.value = False
        except Exception as e:
            errors += 1
            print(f"I2C ERROR: {e}")

print(f"Walk complete. I2C errors: {errors}")
GPIO.cleanup()
```

- [ ] Zero I²C errors during full 48-pin walk
- [ ] No false sensor triggers from I²C activity

---

### Test 3.2 — LED + Relay Simultaneous

Light 16 LEDs on MCP #1 and #2, pulse a relay on MCP #3.

- [ ] LED brightness does NOT dip when relay energises
- [ ] Relay operation does not cause I²C errors
- [ ] LEDs remain stable during relay click

---

### Test 3.3 — Sensor Noise Immunity

With all LEDs lit and relay pulsing repeatedly, monitor hall sensor outputs.

- [ ] No false triggers from relay switching
- [ ] No false triggers from LED switching

> If false triggers occur, solder a 100nF ceramic capacitor across VCC–GND as close to each KY-024 module as possible.

- [ ] Capacitor fix applied? [ ] yes / [ ] not needed

---

### Test 3.4 — Power Consumption Measurement

Measure supply current with everything running (all LEDs lit, relay energised, sensors powered).

| Supply | Rated Capacity | Measured Current | Headroom | Pass |
|---|---|---|---|---|
| 5V (Pi + logic + relays) | ___ A | ___ A | ___% | [ ] |
| 12V (LEDs) | ___ A | ___ A | ___% | [ ] |

- [ ] Both supplies have at least 20% headroom below capacity

---

### Test 3.5 — Continuous Soak Test (30+ Minutes)

Run all subsystems cycling continuously. Log everything.

```python
import board, busio, time, logging
import RPi.GPIO as GPIO
from adafruit_mcp230xx.mcp23017 import MCP23017
from digitalio import Direction

logging.basicConfig(filename='soak_test.log', level=logging.INFO,
    format='%(asctime)s %(levelname)s %(message)s')

i2c = busio.I2C(board.SCL, board.SDA)
boards = [MCP23017(i2c, address=0x20), MCP23017(i2c, address=0x21), MCP23017(i2c, address=0x22)]

GPIO.setmode(GPIO.BCM)
for sp in [4, 5, 6, 7, 8, 9]:
    GPIO.setup(sp, GPIO.IN, pull_up_down=GPIO.PUD_UP)

errors = 0
false_triggers = 0
cycles = 0

try:
    while True:
        cycles += 1
        state = (cycles % 2 == 0)
        for mcp in boards:
            for p in range(16):
                try:
                    mcp.get_pin(p).direction = Direction.OUTPUT
                    mcp.get_pin(p).value = state
                except Exception as e:
                    errors += 1
                    logging.error(f"I2C error cycle {cycles}: {e}")

        for sp in [4, 5, 6, 7, 8, 9]:
            if GPIO.input(sp) == 0:
                false_triggers += 1
                logging.warning(f"Sensor GPIO {sp} triggered at cycle {cycles}")

        if cycles % 10 == 0:
            relay = boards[2].get_pin(8)
            relay.direction = Direction.OUTPUT
            relay.value = False
            time.sleep(0.3)
            relay.value = True

        if cycles % 100 == 0:
            msg = f"Cycle {cycles} | errors: {errors} | false triggers: {false_triggers}"
            print(msg)
            logging.info(msg)

        time.sleep(0.5)

except KeyboardInterrupt:
    summary = f"SOAK COMPLETE: {cycles} cycles, {errors} I2C errors, {false_triggers} false triggers"
    print(summary)
    logging.info(summary)
    GPIO.cleanup()
```

- [ ] Soak test ran for at least 30 minutes
- [ ] Total cycles completed: ___
- [ ] I²C errors: ___ (target: 0)
- [ ] False sensor triggers: ___ (target: 0)
- [ ] No thermal issues (boards, transistors, PSU all cool to touch)

---

## 4. PASS CRITERIA

All of the following must be true before proceeding to **Stage 7**:

- [ ] Zero I²C errors over 30-minute soak
- [ ] No false sensor triggers from relay or LED switching
- [ ] Both power supplies within capacity with 20%+ headroom
- [ ] All subsystems operate simultaneously without interference

---

## 5. MEASURED VALUES

| Item | Value |
|---|---|
| Soak duration | ___ minutes |
| Total cycles | |
| I²C errors | |
| False triggers | |
| 5V supply current (peak) | ___ A |
| 12V supply current (peak) | ___ A |

---

*Tsana Forge — RAIL CONTROL | Hardware Testing Regime Stage 6 | v1.0 | https://tsana.net*
