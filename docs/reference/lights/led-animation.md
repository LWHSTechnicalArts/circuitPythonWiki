# adafruit_led_animation

!!! info "Works with"
    Any board. Requires a NeoPixel or DotStar strip or ring already configured as a pixel object.

## What it does
Provides a library of pre-built animation effects — comets, chases, pulses, rainbows, sparkles, and more — for NeoPixel and DotStar LED strips. Instead of writing your own timing and color math, you instantiate an animation class and call `animate()` in your main loop. The library handles frame timing internally so animations run at a consistent speed regardless of other code in the loop.

## Installing the library
Copy the `adafruit_led_animation/` folder and `adafruit_pixelbuf.mpy` from the bundle's `lib/` folder to your board's `lib/` folder.

## Quick start
```python
import board
import neopixel
from adafruit_led_animation.animation.comet import Comet
from adafruit_led_animation.color import RED

# Set up pixels with auto_write=False — the animation library controls updates
pixels = neopixel.NeoPixel(board.D6, 30, brightness=0.3, auto_write=False)

# Create a comet animation: tail_length controls the fade trail
comet = Comet(pixels, speed=0.02, color=RED, tail_length=10, bounce=True)

while True:
    comet.animate()  # call every loop iteration; the library handles timing
```

## Key things you can do
| What you want | How to do it |
|---------------|--------------|
| Solid blinking | `Blink(pixels, speed=0.5, color=BLUE)` |
| Moving dot with tail | `Comet(pixels, speed=0.02, color=GREEN, tail_length=8)` |
| Theater-chase pattern | `Chase(pixels, speed=0.1, color=WHITE, size=3)` |
| Breathing pulse | `Pulse(pixels, speed=0.1, period=3, color=PURPLE)` |
| Full rainbow | `Rainbow(pixels, speed=0.1, period=2)` |
| Random sparkle | `Sparkle(pixels, speed=0.05, color=GOLD, num_sparkles=10)` |
| Cycle through colors | `ColorCycle(pixels, speed=0.4, colors=[RED, GREEN, BLUE])` |
| Run animations in sequence | `AnimationSequence(anim1, anim2, advance_interval=5)` |
| Run animations simultaneously | `AnimationGroup(anim1, anim2)` |

!!! note "Performance tip"
    Always pass `auto_write=False` when creating your pixel object. The animation library calls `show()` itself at the right time. Using the default `auto_write=True` causes redundant hardware writes and slows down animations.

## Reading the official docs
[https://docs.circuitpython.org/projects/led-animation/en/latest/](https://docs.circuitpython.org/projects/led-animation/en/latest/)

The API reference lists every constructor parameter for each animation class — pay attention to `speed`, `period`, `tail_length`, `bounce`, and `reverse`, which vary by class. The `AnimationSequence` and `AnimationGroup` pages explain how to layer and sequence multiple effects, including syncing groups to the same timing.

## Projects using this library
- [CircuitPython LED Animations](https://learn.adafruit.com/circuitpython-led-animations/overview) — The primary guide; covers every built-in animation class with wiring diagrams and full code examples. *Credit: Adafruit Learning System*
- [Stomp Reactive Light-Up Slippers](https://learn.adafruit.com/stomp-reactive-light-up-slippers) — Wearable project that triggers LED animations from a pressure sensor in a slipper sole. *Credit: Adafruit Learning System*
- [Close Encounters of the MIDI NeoPixel Visualizer Kind](https://learn.adafruit.com/close-encounters-of-the-midi-neopixel-visualizer-kind) — Drives LED animations in sync with incoming MIDI note data. *Credit: Adafruit Learning System*
