# HC-SR04

!!! info "Works with"
    Any CircuitPython board with digital GPIO

!!! warning "Voltage compatibility"
    The standard HC-SR04 runs on 5V logic and will damage 3.3V boards (Trinket M0, Feather, etc.). Use the **HC-SR04P** variant instead — it works at both 3.3V and 5V.

## What it does

The HC-SR04 measures distance by emitting an ultrasonic sound pulse and timing how long the echo takes to return. It covers a range of 2cm to 400cm and is one of the cheapest and most widely available distance sensors available. Good for obstacle detection, liquid level measurement, and proximity triggers where centimeter-level accuracy is acceptable.

## Installing the library

Download `adafruit_hcsr04.mpy` from the [Adafruit CircuitPython Bundle](https://circuitpython.org/libraries) and copy it to the `lib/` folder on your board.

## Quick start

```python
import time
import board
import adafruit_hcsr04

sonar = adafruit_hcsr04.HCSR04(trigger_pin=board.D2, echo_pin=board.D3)

while True:
    try:
        print(f"Distance: {sonar.distance:.1f} cm")
    except RuntimeError:
        print("Out of range")
    time.sleep(0.1)
```

## Key things you can do

| What you want | How to do it |
|---------------|--------------|
| Read distance | `sonar.distance` — returns cm as a float |
| Handle out-of-range readings | Wrap in `try/except RuntimeError` |
| Change the trigger and echo pins | Set `trigger_pin` and `echo_pin` in the constructor |
| Adjust timeout | `sonar = adafruit_hcsr04.HCSR04(trigger_pin=..., echo_pin=..., timeout=0.1)` — seconds |
| Use with 3.3V boards | Use the HC-SR04P variant — same wiring, 3.3V safe |

## Reading the official docs

Full API reference: [https://docs.circuitpython.org/projects/hcsr04/en/latest/](https://docs.circuitpython.org/projects/hcsr04/en/latest/)

Key things to look up:

- `RuntimeError` is raised when the echo does not return within the timeout — this happens when the target is out of range, the angle is too oblique, or the surface absorbs sound
- The timeout parameter (in seconds) controls how long the library waits for the echo; the default is generous but can be reduced to speed up the loop
- Soft surfaces and angles beyond about 15 degrees from perpendicular produce unreliable readings

## Projects using this library

- [Adafruit ultrasonic sonar guide](https://learn.adafruit.com/ultrasonic-sonar-distance-sensors/python-circuitpython) — wiring for 5V and 3.3V boards, usage examples
- This wiki's Distance Alert project — uses HC-SR04P readings to trigger a buzzer and NeoPixel
