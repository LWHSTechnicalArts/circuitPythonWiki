# simpleio — tone()

!!! info "Works with"
    Any board with a PWM-capable pin. Connect a passive piezo buzzer or a small speaker (with appropriate current-limiting resistor) between the pin and ground.

## What it does
The `tone()` function in the `simpleio` library generates a square-wave tone at a specified frequency on any PWM output pin. It is the simplest way to produce musical notes or beeps in CircuitPython — one function call plays a note for a given duration and returns when finished. For more complex audio needs (WAV playback, mixing), see the `audioio` and `audiomixer` built-in modules.

## Installing the library
Copy `simpleio.mpy` from the bundle's `lib/` folder to your board's `lib/` folder.

## Quick start
```python
import board
import simpleio
import time

# Play A4 (440 Hz) for half a second
simpleio.tone(board.D4, 440, duration=0.5)

# Play a simple scale using note frequencies
notes = [262, 294, 330, 349, 392, 440, 494, 523]  # C4 through C5
for freq in notes:
    simpleio.tone(board.D4, freq, duration=0.2)
    time.sleep(0.05)  # brief gap between notes
```

## Key things you can do
| What you want | How to do it |
|---------------|--------------|
| Play a single tone | `simpleio.tone(pin, frequency, duration=seconds)` |
| Play middle C | `simpleio.tone(board.D4, 262)` |
| Play a rest (silence) | `time.sleep(duration)` — just delay without calling tone |
| Non-blocking tone | Use `pwmio.PWMOut` directly; set `frequency` and `duty_cycle=0x8000` to start, `0` to stop |
| Common note frequencies | C4=262, D4=294, E4=330, F4=349, G4=392, A4=440, B4=494, C5=523 |

!!! note "Blocking behavior"
    `simpleio.tone()` blocks until the duration has elapsed — no other code runs while the note plays. For non-blocking tone generation (e.g., playing a note while also reading a sensor), use `pwmio.PWMOut` directly: set `duty_cycle = 32768` to start the tone and `duty_cycle = 0` to stop it on your own schedule.

## Reading the official docs
[https://docs.circuitpython.org/projects/simpleio/en/latest/](https://docs.circuitpython.org/projects/simpleio/en/latest/)

The simpleio docs cover the full module, which includes helpers beyond `tone()` — `map_range()` for value scaling and `DigitalIO` helpers. The `tone()` entry documents the pin, frequency, and duration parameters. For a complete note-frequency table, the CircuitPython Essentials guide linked below includes a piano keyboard chart.

## Projects using this library
- [Make It Sound](https://learn.adafruit.com/make-it-sound) — Covers all CircuitPython audio output options including simpleio tones, with wiring diagrams for buzzers and speakers. *Credit: Adafruit Learning System*
- [CircuitPython Essentials: PWM](https://learn.adafruit.com/circuitpython-essentials/circuitpython-pwm) — Explains the underlying `pwmio` module and shows both `simpleio.tone()` and direct PWM tone generation. *Credit: Adafruit Learning System*
