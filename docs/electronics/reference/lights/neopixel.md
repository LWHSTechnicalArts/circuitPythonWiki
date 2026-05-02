# NeoPixel

!!! info "Works with"
    Any CircuitPython board with a digital output pin

---

## What it does

NeoPixels (also called WS2812B LEDs) are individually addressable RGB LEDs. Each one has its own color controller chip inside, and you control all of them through a single data wire. That means you can have a strip of 30 LEDs and set each one to a completely different color — all from one pin on your board.

The `neopixel` library gives you a straightforward way to work with these LEDs. You can set the color of any single pixel by its position (index), fill all pixels with one color at once, and adjust the overall brightness of the entire strip.

---

## Installing the library

Copy `neopixel.mpy` from the bundle's `lib/` folder to the `lib/` folder on your board.

Not sure how to install a library? See [How to install a library](../index.md#how-to-install-a-library).

---

## Quick start

```python
import board
import neopixel

pixels = neopixel.NeoPixel(board.D4, 8, brightness=0.3)

pixels.fill((255, 0, 0))  # all red
pixels[0] = (0, 255, 0)   # first pixel green
```

What this does line by line:

- `neopixel.NeoPixel(board.D4, 8, brightness=0.3)` — creates a NeoPixel object connected to pin D4, controlling 8 pixels, at 30% brightness
- `pixels.fill((255, 0, 0))` — sets every pixel to red (r=255, g=0, b=0)
- `pixels[0] = (0, 255, 0)` — sets the first pixel (index 0) to green, leaving the rest red

Colors are RGB tuples: three numbers between 0 and 255 representing red, green, and blue. `(255, 255, 255)` is white. `(0, 0, 0)` is off.

---

## Key things you can do

| What you want | How to do it |
|---|---|
| Fill all pixels one color | `pixels.fill((r, g, b))` |
| Set one pixel by index | `pixels[n] = (r, g, b)` |
| Turn everything off | `pixels.fill((0, 0, 0))` |
| Adjust brightness | `pixels.brightness = 0.5` |
| Use color tuples directly | Pass `(r, g, b)` values — e.g. `(255, 165, 0)` for orange |
| Use RGBW pixels | Add `bpp=4` to the constructor and use `(r, g, b, w)` tuples |

---

## Reading the official docs

The official documentation for this library is at:

[docs.circuitpython.org/projects/neopixel/en/latest/](https://docs.circuitpython.org/projects/neopixel/en/latest/)

The docs show you the `NeoPixel` class constructor and all the parameters it accepts. Once you have the basics working, two parameters are worth knowing about:

**`auto_write`** — By default, any change you make to a pixel updates the strip immediately. If you set `auto_write=False`, changes are held until you call `pixels.show()`. This matters when you are updating many pixels at once: without it, you might see a partial update flash on the strip before all pixels are set. Use `pixels.show()` to push all your changes at the same time.

```python
pixels = neopixel.NeoPixel(board.D4, 8, brightness=0.3, auto_write=False)
pixels[0] = (255, 0, 0)
pixels[1] = (0, 255, 0)
pixels[2] = (0, 0, 255)
pixels.show()  # all three update at once
```

**`pixel_order`** — Most NeoPixels use RGB color order, but some strips wire the colors differently (GRB is common). If your colors look wrong — red looks green, green looks red — try `pixel_order=neopixel.GRB`. The docs list all the available options.

---

## Projects using this library

- [Adafruit NeoPixel Uberguide](https://learn.adafruit.com/adafruit-neopixel-uberguide/python-circuitpython) — the definitive guide to wiring, power, and code for NeoPixels of all shapes and sizes. *Credit: Adafruit Learning System*
- [CircuitPython LED Animations](https://learn.adafruit.com/circuitpython-led-animations/overview) — chase, comet, pulse, rainbow, sparkle and more, all built on top of neopixel. *Credit: Adafruit Learning System*
- [Stomp-Reactive Light Up Slippers](https://learn.adafruit.com/stomp-reactive-light-up-slippers) — wearable NeoPixels that react to footsteps with motion sensing. *Credit: Adafruit Learning System*
