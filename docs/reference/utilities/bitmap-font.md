# adafruit_bitmap_font

!!! info "Works with"
    Any board using `displayio` — Feather M4, Metro M4, PyPortal, Raspberry Pi Pico with a display, and more.

## What it does

`adafruit_bitmap_font` loads proportional bitmap fonts from `.bdf` or `.pcf` font files stored on your CIRCUITPY drive. This gives you control over typography on displays: different sizes, different styles, fonts with descenders and correct letter spacing.

Without this library, you're limited to the built-in `terminalio.FONT`, which is a small fixed-width font — fine for simple readouts, but not for a polished UI.

**Font files** are stored in a `fonts/` folder on your CIRCUITPY drive. Adafruit provides a large collection of free `.bdf` fonts, and you can convert others using standard tools.

Use this library together with `adafruit_display_text` to render text on screen.

## Installing the library

Copy the folder to the `lib/` folder on your CIRCUITPY drive:

```
adafruit_bitmap_font/
```

You also need fonts. Download Adafruit's font collection from the [CircuitPython Bundle extras](https://github.com/adafruit/Adafruit_CircuitPython_Bundle) or from individual Adafruit learn guides. Copy `.bdf` files to a `fonts/` folder on your CIRCUITPY drive.

Both are in the [Adafruit CircuitPython Bundle](https://circuitpython.org/libraries).

## Quick start

```python
import board
import displayio
from adafruit_bitmap_font import bitmap_font
from adafruit_display_text import label

display = board.DISPLAY

# Load a font from the fonts/ folder on CIRCUITPY
font = bitmap_font.load_font("/fonts/Arial-12.bdf")

# Create a text label using the font
text_label = label.Label(font, text="Hello, world", color=0xFFFFFF)
text_label.x = 10
text_label.y = 30

group = displayio.Group()
group.append(text_label)
display.root_group = group

while True:
    pass
```

## Key things you can do

| What you want | How to do it |
|---|---|
| Load a font | `bitmap_font.load_font("/fonts/MyFont-12.bdf")` |
| Use it with a label | `label.Label(font, text="...", color=0xFFFFFF)` |
| Use the built-in font instead | `import terminalio; label.Label(terminalio.FONT, ...)` |
| Load a PCF font | Same call — `load_font()` supports both `.bdf` and `.pcf` |
| Reduce RAM use | Use a smaller point size, or use `terminalio.FONT` |
| Preload glyphs | `font.load_glyphs(b"ABCDEFabcdef0123456789")` — loads only what you need |

### Memory note

Fonts use RAM, which is limited on smaller boards. A 24pt font can use 30KB or more. On a Trinket M0 (32KB RAM total), stick to `terminalio.FONT` or a small 8–10pt `.bdf` file. On a Feather M4 or Metro M4 (192KB RAM), larger fonts are fine.

## Reading the official docs

Full API reference and font conversion tools:
[https://docs.circuitpython.org/projects/bitmap-font/en/latest/](https://docs.circuitpython.org/projects/bitmap-font/en/latest/)

## Projects using this library

- [CircuitPython Display Support Using displayio](https://learn.adafruit.com/circuitpython-display-support-using-displayio) — comprehensive guide to displays, text, and layout in CircuitPython
