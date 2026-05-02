# Your First NeoPixel

!!! info "Works with"
    Any CircuitPython board with a free digital output pin

## What you'll build

You'll wire up a strip of NeoPixel LEDs and write a short CircuitPython script that cycles them through red, green, and blue — a satisfying first step toward building light shows, wearables, and reactive displays.

## What you'll need

- Any CircuitPython board (Trinket M0, Feather, Circuit Playground, etc.)
- A NeoPixel strip or ring (or the built-in NeoPixel on boards that have one)
- A USB cable
- Short jumper wires

## Wiring

```mermaid
graph LR
    Board["Your Board"]
    NP["NeoPixel Strip"]
    Board -->|"Pin 1 → DIN"| NP
    Board -->|"3.3V or 5V → 5V"| NP
    Board -->|"GND → GND"| NP
```

On Trinket M0, use Pin 4 for the data line. On Circuit Playground boards, NeoPixels are built in — no wiring needed, and you can skip this section entirely.

## The code

```python
import board
import neopixel
import time

# Change this to match how many NeoPixels you have
NUM_PIXELS = 8
# Change this to whatever pin your data line is connected to
pixel_pin = board.D4

pixels = neopixel.NeoPixel(pixel_pin, NUM_PIXELS, brightness=0.3)

while True:
    pixels.fill((255, 0, 0))   # red
    time.sleep(0.5)
    pixels.fill((0, 255, 0))   # green
    time.sleep(0.5)
    pixels.fill((0, 0, 255))   # blue
    time.sleep(0.5)
```

## How it works

NeoPixels are individually addressable RGB LEDs. Each pixel contains a tiny color controller chip, which means a single data wire is all you need to set the color of every LED in a strip independently — no matter how long the strip is. The board sends color instructions down the line one pixel at a time, at very high speed.

`neopixel.NeoPixel()` creates an object that represents your entire strip as a list. Each item in the list corresponds to one physical LED. You can set all of them at once using `fill()`, or target individual pixels by index — `pixels[0]` is the first LED, `pixels[1]` is the second, and so on.

Colors are described as `(R, G, B)` tuples, where each value is a number from 0 to 255 controlling how much red, green, or blue light to mix in. `(255, 0, 0)` is full red. `(255, 255, 0)` is yellow. `(0, 0, 0)` turns the pixel off. The `brightness` parameter (set to `0.3` here) scales the overall output — 1.0 is maximum brightness, 0.0 is off. Keeping it below full brightness also reduces heat and current draw, which matters when you're powering from USB.

## Installing the library

The `neopixel` library does not come built into CircuitPython — you need to copy it onto your board manually.

1. Download the CircuitPython library bundle from the Adafruit website. See [Getting Started](../../getting-started.md) for download instructions and bundle details.
2. Inside the bundle, open the `lib/` folder and find the file named `neopixel.mpy`.
3. Copy `neopixel.mpy` into the `lib/` folder on your CircuitPython board (the drive called `CIRCUITPY`).

Once the file is in place, the `import neopixel` line in your code will work.

## Remix it

!!! tip "Remix idea"
    What if each pixel was a different color? Try setting `pixels[0]`, `pixels[1]`, `pixels[2]` individually instead of using `fill()`.
    → [LED Animations](builder-animations.md) shows how to create chases and patterns.

!!! tip "Remix idea"
    What if the color changed based on a sensor reading?
    → [Temperature Lamp](../sensors/starter-temperature-lamp.md) maps temperature to NeoPixel color.

!!! tip "Remix idea"
    What if the lights reacted to sound or motion?
    → [Reactive Wearable](hacker-reactive-wearable.md) takes lights to the next level.

## Go deeper

- [NeoPixel Reference](../../reference/lights/neopixel.md) — all the library methods and options
- [Adafruit NeoPixel Überguide](https://learn.adafruit.com/adafruit-neopixel-uberguide/python-circuitpython) — comprehensive guide including power and wiring advice. *Credit: Adafruit Learning System*
