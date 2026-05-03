# simpleio

!!! info "Works with"
    Any CircuitPython board.

## What it does

`simpleio` is a grab-bag of helper functions for things you want to do often but that require more code than they should. It doesn't do any one big thing — it just makes a handful of common tasks shorter and clearer.

The functions you'll reach for most:

- **`tone()`** — play a tone through a buzzer or speaker pin
- **`map_range()`** — rescale a number from one range to another
- **`DigitalIO` helpers** — quick digital reads without setup boilerplate

## Installing the library

Copy this to the `lib/` folder on your CIRCUITPY drive:

```
simpleio.mpy
```

Available in the [Adafruit CircuitPython Bundle](https://circuitpython.org/libraries).

## Quick start

```python
import board
import simpleio

# Play a 440 Hz tone for 0.5 seconds on pin A0
simpleio.tone(board.A0, 440, duration=0.5)

# Map a sensor value from one range to another
# Example: temperature 15–35 C mapped to LED brightness 0–255
temp = 24.0
brightness = simpleio.map_range(temp, 15, 35, 0, 255)
print(int(brightness))  # 229

# Map an analog read (0–65535) to a servo angle (0–180)
import analogio
pot = analogio.AnalogIn(board.A1)
angle = simpleio.map_range(pot.value, 0, 65535, 0, 180)
```

`map_range()` clamps the output to the target range, so you don't have to worry about out-of-bounds values from noisy sensors.

## Key things you can do

| What you want | How to do it |
|---|---|
| Play a tone | `simpleio.tone(pin, frequency, duration=1.0)` |
| Rescale a value | `simpleio.map_range(x, in_min, in_max, out_min, out_max)` |
| Map potentiometer to brightness | `map_range(pot.value, 0, 65535, 0, 255)` |
| Map temperature to a color | `map_range(temp, 0, 100, 0, 65535)` |
| Quick digital read | `simpleio.DigitalIO(pin)` |
| Shift out data | `simpleio.shift_out(data_pin, clock_pin, value)` |

## Reading the official docs

Full API reference including shift register and servo helpers:
[https://docs.circuitpython.org/projects/simpleio/en/latest/](https://docs.circuitpython.org/projects/simpleio/en/latest/)

## Projects using this library

- [Make It Sound](https://learn.adafruit.com/make-it-sound) — tone generation and audio output with CircuitPython
