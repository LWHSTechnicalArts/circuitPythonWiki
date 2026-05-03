# adafruit_pixel_framebuf

!!! info "Works with"
    Any board. Requires a NeoPixel or DotStar grid, matrix, or strip arranged in a rectangular layout.

## What it does
Wraps a NeoPixel or DotStar pixel object in a framebuffer interface so you can draw graphics primitives — lines, rectangles, circles, and text — instead of addressing individual pixels by index. It builds on `adafruit_framebuf` to provide the drawing API, then maps the resulting frame onto your physical LED layout. This is particularly useful for LED matrix panels, grids, and flexible matrices.

## Installing the library
Copy `adafruit_pixel_framebuf.mpy` and `adafruit_framebuf.mpy` from the bundle's `lib/` folder to your board's `lib/` folder.

## Quick start
```python
import board
import neopixel
from adafruit_pixel_framebuf import PixelFramebuffer, HORIZONTAL

# A 32x8 NeoPixel matrix wired in rows (HORIZONTAL orientation)
pixels = neopixel.NeoPixel(board.D6, 32 * 8, brightness=0.1, auto_write=False)

fb = PixelFramebuffer(pixels, 32, 8, orientation=HORIZONTAL)

# Draw a filled rectangle
fb.fill_rect(0, 0, 10, 8, 0xFF0000)   # red bar on the left

# Draw a line
fb.line(0, 0, 31, 7, 0x00FF00)        # green diagonal

# Render text (requires a font; uses built-in 5x8 font by default)
fb.text("Hi!", 1, 0, 0xFFFFFF)

# Push the frame to the LEDs
fb.display()
```

## Key things you can do
| What you want | How to do it |
|---------------|--------------|
| Fill entire display | `fb.fill(color)` |
| Set one pixel | `fb.pixel(x, y, color)` |
| Draw a line | `fb.line(x0, y0, x1, y1, color)` |
| Draw a rectangle | `fb.rect(x, y, w, h, color)` or `fb.fill_rect(...)` |
| Draw a circle | `fb.circle(x, y, radius, color)` |
| Draw text | `fb.text("string", x, y, color)` |
| Scroll contents | `fb.scroll(dx, dy)` |
| Send frame to LEDs | `fb.display()` |
| Set wiring direction | Pass `orientation=HORIZONTAL` or `orientation=VERTICAL` at init |
| Reverse row direction | Pass `alternating=True` for serpentine (zigzag) wiring |

## Reading the official docs
[https://docs.circuitpython.org/projects/pixel-framebuf/en/latest/](https://docs.circuitpython.org/projects/pixel-framebuf/en/latest/)

The constructor reference explains the `orientation`, `alternating`, and `reverse_x` / `reverse_y` parameters, which you will need to set correctly for your specific matrix wiring. The `adafruit_framebuf` drawing method reference (linked from the pixel-framebuf docs) covers every drawing primitive in detail.

## Projects using this library
- [Easy NeoPixel Graphics with the CircuitPython Pixel Framebuf Library](https://learn.adafruit.com/easy-neopixel-graphics-with-the-circuitpython-pixel-framebuf-library/overview) — The primary guide; covers matrix wiring, orientation settings, and all drawing methods. *Credit: Adafruit Learning System*
- [Driving TM1814 Addressable LEDs](https://learn.adafruit.com/driving-tm1814-addressable-leds/circuitpython-led-animations) — Shows pixel-framebuf used with a different addressable LED type. *Credit: Adafruit Learning System*
