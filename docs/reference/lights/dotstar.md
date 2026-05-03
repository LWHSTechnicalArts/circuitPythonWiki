# adafruit_dotstar

!!! info "Works with"
    Any board with SPI or any two digital output pins. Built into Trinket M0, Gemma M0, and ItsyBitsy M0 Express as an onboard single DotStar LED.

## What it does
Controls APA102 (DotStar) addressable RGB LEDs over a two-wire interface (clock + data). Because the clock line is separate from the data line, DotStars update faster and more reliably than NeoPixels, making them well-suited for high-speed animations or long strips. The library manages a pixel buffer, color ordering, and global brightness.

## Installing the library
Copy `adafruit_dotstar.mpy` from the bundle's `lib/` folder to your board's `lib/` folder.

## Quick start
```python
import board
import adafruit_dotstar

# Number of pixels in the strip
NUM_PIXELS = 30

# Use hardware SPI pins (MOSI/SCK) for best performance
# For bit-bang on arbitrary pins: adafruit_dotstar.DotStar(board.D1, board.D0, NUM_PIXELS)
pixels = adafruit_dotstar.DotStar(board.SCK, board.MOSI, NUM_PIXELS, brightness=0.2)

# Set a single pixel: index, (R, G, B)
pixels[0] = (255, 0, 0)   # red

# Fill the whole strip with one color
pixels.fill((0, 0, 255))  # blue

# Changes are sent to the LEDs immediately by default (auto_write=True)
# Set auto_write=False and call pixels.show() manually for animations
```

## Key things you can do
| What you want | How to do it |
|---------------|--------------|
| Set one pixel | `pixels[n] = (r, g, b)` |
| Set all pixels | `pixels.fill((r, g, b))` |
| Adjust brightness | `pixels.brightness = 0.5` (0.0–1.0) |
| Turn everything off | `pixels.fill((0, 0, 0))` |
| Manual refresh | `pixels.auto_write = False` then `pixels.show()` |
| Change color order | Pass `pixel_order=adafruit_dotstar.BGR` at init |
| Use onboard DotStar | `adafruit_dotstar.DotStar(board.APA102_SCK, board.APA102_MOSI, 1)` |

## Reading the official docs
[https://docs.circuitpython.org/projects/dotstar/en/latest/](https://docs.circuitpython.org/projects/dotstar/en/latest/)

The API reference section lists every parameter for `DotStar.__init__()`, including `pixel_order` options (RGB, RBG, GRB, BGR, etc.) and the `auto_write` flag. Check there when you need fine-grained control over color ordering for a specific strip that arrived with colors swapped, or when tuning animation frame rates by disabling auto-write.

## Projects using this library
- [Adafruit DotStar LEDs](https://learn.adafruit.com/adafruit-dotstar-leds/python-circuitpython) — Complete guide to wiring and driving DotStar strips and rings with CircuitPython. *Credit: Adafruit Learning System*
- [CircuitPython Essentials: DotStar](https://learn.adafruit.com/circuitpython-essentials/circuitpython-dotstar) — Covers the onboard DotStar on M0 boards and basic color control. *Credit: Adafruit Learning System*
- [CircuitPython LED Animations](https://learn.adafruit.com/circuitpython-led-animations/overview) — Uses DotStars (and NeoPixels) as the target hardware for the adafruit_led_animation library. *Credit: Adafruit Learning System*
