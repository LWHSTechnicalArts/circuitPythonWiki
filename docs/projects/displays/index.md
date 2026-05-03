# Displays

Your board has been running code silently this whole time. It blinks LEDs, reads sensors, and makes decisions — but it never tells you anything directly. Add a display and that changes completely. Now your project can show sensor readings, draw graphics, count down to an event, or pull live data from the internet and put it on screen.

That shift — from a device that acts to one that communicates — opens up a huge range of projects. Displays are also one of the best debugging tools you have. Instead of guessing what your code is doing, you can just read it off the screen.

---

## The display landscape

CircuitPython supports three main categories of displays, each with a different sweet spot.

### OLED (starter territory)

OLED displays are small, cheap, and talk over I2C — the same two-wire bus you probably already know. The SSD1306 128x64 monochrome OLED is the standard starting point. It draws white pixels on a black background, has enough room for several lines of text, and works with nearly every CircuitPython board. No extra libraries for the bus, no complicated wiring.

### TFT LCD (builder territory)

TFT displays use SPI for speed and give you full color. The ST7789 and ILI9341 are the most common drivers. You can draw filled shapes, display bitmap images, and render styled text at high refresh rates. The tradeoff is slightly more complex wiring and a larger library footprint, but the results look genuinely impressive.

### LED matrices (retro effects)

An 8x8 LED matrix driven by the HT16K33 gives you 64 individually addressable LEDs over I2C. The resolution is low by design, which makes it perfect for pixel art, scrolling messages, and visual effects that feel deliberately retro. The low pixel count is a feature when you want a big, bold, visible output.

---

## CircuitPython's displayio system

CircuitPython uses a unified display framework called `displayio`. Instead of drawing pixels directly, you build a tree of objects — groups, tilemaps, labels — and attach them to a display. When you update an object in the tree, `displayio` handles redrawing automatically.

This matters because it means the same code structure works whether you're targeting a 128x64 OLED or a full-color 320x240 TFT. The display driver changes; the way you compose the screen does not.

Fonts in CircuitPython are separate `.bdf` or precompiled `.pcf` files. The `adafruit_display_text` library loads them and renders text as a `Label` object that you can position anywhere on screen. This is more flexible than fixed-position text but does require keeping the font files on your board.

---

## Projects in this section

The projects below follow a Starter → Builder → Hacker progression. Start with the one that matches your current experience level, or jump straight to whatever looks most interesting.

| Level | Project | What you learn |
|-------|---------|----------------|
| Starter | [OLED Hello World](starter-oled-hello.md) | displayio basics, I2C, text labels |
| Starter | [Scrolling Text on an LED Matrix](starter-led-matrix.md) | HT16K33, pixel addressing, brightness |
| Builder | [Drawing on a Color TFT Screen](builder-tft-graphics.md) | SPI displays, shapes, groups |
| Builder | [Countdown Clock](builder-countdown-clock.md) | NTP time sync, WiFi, live display updates |
| Builder | [Digital Compass](builder-compass.md) | IMU sensor, math for graphics, round display |
| Hacker | [IoT Dashboard with PyPortal](hacker-pyportal-dashboard.md) | JSON APIs, touchscreen, full-stack hardware |

---

## Which display should I buy?

If you are just starting out, get an SSD1306 128x64 OLED breakout. It is the cheapest option, the wiring is minimal, and every example in the Adafruit Learning System supports it. Once you have that working, a 1.14" ST7789 TFT is the natural next step for color.

If you want to go straight to an all-in-one device, the PyPortal has a TFT display, WiFi, a speaker, and a touchscreen built onto a single board. It costs more, but you skip all the wiring.
