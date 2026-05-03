# adafruit_display_text

!!! info "Works with"
    Any board using CircuitPython's `displayio` framework. Works with any display that has a `displayio` driver — SSD1306, ST7789, ILI9341, and all others.

## What it does
Adds text rendering to `displayio` scenes. Without this library, displayio has no way to show text. The library provides `Label` (single- or multi-line text with optional background color) and `ScrollingLabel` (auto-scrolling text for long strings). It supports the tiny built-in `terminalio.FONT` with no file required, or larger proportional fonts loaded from `.bdf` or `.pcf` font files stored on the CIRCUITPY drive.

## Installing the library
Copy the `adafruit_display_text/` folder from the bundle's `lib/` folder to your board's `lib/` folder.

## Quick start
```python
import board
import displayio
import terminalio
from adafruit_display_text import label

# Assumes 'display' is already initialized (e.g., board.DISPLAY or your driver)
splash = displayio.Group()
display.root_group = splash

# Basic label using the built-in 5x8 terminal font
lbl = label.Label(
    terminalio.FONT,
    text="Hello world",
    color=0xFFFFFF,         # text color (RGB hex)
    background_color=None,  # transparent background
    x=10,
    y=30,
)
splash.append(lbl)

# Update text later
lbl.text = "Updated!"

# Position using anchor_point + anchored_position to center
lbl.anchor_point = (0.5, 0.5)               # anchor at center of label
lbl.anchored_position = (display.width // 2, display.height // 2)
```

## Key things you can do
| What you want | How to do it |
|---------------|--------------|
| Show text | `label.Label(font, text="...", color=0xRRGGBB)` |
| Change text dynamically | `lbl.text = "new string"` |
| Add background color | `label.Label(..., background_color=0x000000)` |
| Scale up text | `label.Label(..., scale=2)` |
| Multi-line text | Include `\n` in the string; set `line_spacing=1.25` |
| Word-wrap text | `label.Label(..., word_wrap=True)` — wrap to `line_spacing` width |
| Use a larger font | Load with `adafruit_bitmap_font.bitmap_font.load_font("/fonts/myfont.bdf")` |
| Scroll long text | `from adafruit_display_text.scrolling_label import ScrollingLabel` |
| Center or align | Set `anchor_point` (0–1 tuple) and `anchored_position` (pixel coords) |

!!! note "Font options"
    `terminalio.FONT` is always available and uses no storage space, but it is a fixed 5x8 pixel font — good for data readouts, not readable at small sizes. Larger, more legible fonts can be downloaded from the `adafruit/circuitpython-fonts` GitHub repository and placed in a `/fonts` folder on your CIRCUITPY drive.

## Reading the official docs
[https://docs.circuitpython.org/projects/display-text/en/latest/](https://docs.circuitpython.org/projects/display-text/en/latest/)

The API reference documents every `Label` constructor parameter, including `padding_top/bottom/left/right` for adding space around the text bounding box, and `tab_replacement` for handling tab characters. The `ScrollingLabel` reference explains `max_characters` and `animate()` for building a scrolling ticker.

## Projects using this library
- [CircuitPython Display Support Using displayio](https://learn.adafruit.com/circuitpython-display-support-using-displayio) — The main displayio guide; includes a full section on text labels and font loading. *Credit: Adafruit Learning System*
