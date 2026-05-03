# adafruit_fancyled

!!! info "Works with"
    Any board. Designed to work alongside `neopixel` or `adafruit_dotstar` — it handles color math and you pass the results to your pixel object.

## What it does
Provides advanced color manipulation tools for NeoPixels and DotStars, inspired by the FastLED Arduino library. It adds HSV color model support, perceptually linear gamma correction, color palette lookups, and blend modes — none of which are in the basic neopixel library. If you want smooth color fades, fire effects, or palette-driven animations that look correct to the human eye, this library supplies the math.

## Installing the library
Copy the `adafruit_fancyled/` folder from the bundle's `lib/` folder to your board's `lib/` folder.

## Quick start
```python
import board
import neopixel
import adafruit_fancyled.adafruit_fancyled as fancy

NUM_PIXELS = 30
pixels = neopixel.NeoPixel(board.D6, NUM_PIXELS, auto_write=False)

# CHSV: hue (0-255), saturation (0-255), value/brightness (0-255)
color_hsv = fancy.CHSV(160, 255, 200)

# CRGB: red, green, blue (0-255 or 0.0-1.0 floats)
color_rgb = fancy.CRGB(255, 128, 0)

# Gamma-correct a color so brightness looks linear to the eye
corrected = fancy.gamma_adjust(color_hsv, gamma_value=2.6)

# Pack to a 24-bit int for use with neopixel
pixels[0] = corrected.pack()

# Define a palette (list of CRGB or CHSV entries)
palette = [fancy.CRGB(255, 0, 0), fancy.CRGB(255, 128, 0), fancy.CRGB(255, 255, 0)]

# Look up an interpolated color from the palette (offset 0.0-1.0)
blended = fancy.palette_lookup(palette, 0.33)
pixels[1] = fancy.gamma_adjust(blended).pack()

pixels.show()
```

## Key things you can do
| What you want | How to do it |
|---------------|--------------|
| Create a color by hue | `fancy.CHSV(hue, saturation, value)` |
| Create a color by RGB | `fancy.CRGB(r, g, b)` |
| Correct for eye perception | `fancy.gamma_adjust(color, gamma_value=2.6)` |
| Look up palette color | `fancy.palette_lookup(palette, offset)` — offset is 0.0–1.0 |
| Convert to pixel-ready int | `color.pack()` |
| Blend two colors | `fancy.mix(color1, color2, weight)` — weight 0.0–1.0 |
| Animate a hue cycle | Increment CHSV hue by a small step each frame |

!!! note "Who needs this"
    For basic solid colors and simple fills, the standard `neopixel` library is all you need. FancyLED is for projects where color accuracy matters — fire simulations, smooth rainbow cycles, or any effect where naive linear math produces muddy or dim-looking transitions.

## Reading the official docs
[https://docs.circuitpython.org/projects/fancyled/en/latest/](https://docs.circuitpython.org/projects/fancyled/en/latest/)

The API reference documents every parameter of `CHSV`, `CRGB`, `gamma_adjust`, `palette_lookup`, and `mix`. Look there for the full list of built-in named palettes and for details on how `palette_lookup` interpolates between palette entries — useful when you want a smooth fire or ocean gradient with minimal code.

## Projects using this library
- [FancyLED Library for CircuitPython](https://learn.adafruit.com/fancyled-library-for-circuitpython) — The primary guide; walks through HSV colors, gamma correction, palettes, and blend modes with examples. *Credit: Adafruit Learning System*
- [Stomp Reactive Light-Up Slippers](https://learn.adafruit.com/stomp-reactive-light-up-slippers) — Uses FancyLED palettes to produce color effects triggered by a pressure sensor. *Credit: Adafruit Learning System*
- [CircuitPython LED Animations](https://learn.adafruit.com/circuitpython-led-animations/overview) — Demonstrates how FancyLED color math integrates with animation library effects. *Credit: Adafruit Learning System*
