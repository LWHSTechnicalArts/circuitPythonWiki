# adafruit_debouncer

!!! info "Works with"
    Any CircuitPython board.

## What it does

When a physical button is pressed, the metal contacts briefly bounce — making and breaking contact many times in a few milliseconds before settling. To a microcontroller running thousands of iterations per second, that looks like the button being pressed dozens of times.

The Debouncer library filters out that noise. It watches an input and only reports a state change after the signal has been stable for a short time (5ms by default).

**The two properties you'll use most:**

- `fell` — `True` for exactly one loop iteration when the pin goes from `True` to `False` (button pressed, with pull-up)
- `rose` — `True` for exactly one loop iteration when the pin goes from `False` to `True` (button released)

## Installing the library

Copy this to the `lib/` folder on your CIRCUITPY drive:

```
adafruit_debouncer.mpy
```

Available in the [Adafruit CircuitPython Bundle](https://circuitpython.org/libraries).

## Quick start

```python
import board
import digitalio
import time
from adafruit_debouncer import Debouncer
from digitalio import Pull

# Set up the raw pin
raw = digitalio.DigitalInOut(board.D2)
raw.direction = digitalio.Direction.INPUT
raw.pull = Pull.UP

# Wrap it in a Debouncer
button = Debouncer(raw)

while True:
    button.update()  # must call this every loop

    if button.fell:
        print("Button pressed")

    if button.rose:
        print("Button released")
```

`button.update()` must be called every loop iteration. That's where the debouncing happens.

## Key things you can do

| What you want | How to do it |
|---|---|
| Detect a press (pull-up wiring) | `if button.fell:` |
| Detect a release | `if button.rose:` |
| Read the current stable state | `button.value` |
| Check how long it's been held | `button.current_duration` — time in seconds since last change |
| Check duration of last state | `button.last_duration` |
| Change the debounce interval | `Debouncer(raw, interval=0.010)` — default is 0.005s |
| Debounce a lambda instead of a pin | `Debouncer(lambda: some_condition)` |

## Reading the official docs

Full API reference including duration tracking:
[https://docs.circuitpython.org/projects/debouncer/en/latest/](https://docs.circuitpython.org/projects/debouncer/en/latest/)

## Projects using this library

- [CircuitPython Essentials](https://learn.adafruit.com/circuitpython-essentials) — covers buttons, inputs, and debouncing as part of the core CircuitPython curriculum
