# adafruit_display_shapes

!!! info "Works with"
    Any board using CircuitPython's `displayio` framework. Works with any display that has a `displayio` driver.

## What it does
Adds geometric shape drawing to `displayio` scenes — rectangles, circles, triangles, rounded rectangles, lines, and polygons. Each shape is a `displayio`-compatible object that you append to a `Group` and position like any other display element. Shapes support independent fill and outline colors and can have transparent fills or borders. This library is the standard way to draw UI chrome — buttons, borders, progress bars, and indicators — in displayio projects.

## Installing the library
Copy the `adafruit_display_shapes/` folder from the bundle's `lib/` folder to your board's `lib/` folder.

## Quick start
```python
import board
import displayio
from adafruit_display_shapes.rect import Rect
from adafruit_display_shapes.circle import Circle

# Assumes 'display' is already initialized
splash = displayio.Group()
display.root_group = splash

# Filled red rectangle at (10, 10), 80 pixels wide, 30 pixels tall
r = Rect(10, 10, 80, 30, fill=0xFF0000)
splash.append(r)

# Circle at center (120, 80), radius 20, no fill, white outline
c = Circle(120, 80, 20, fill=None, outline=0xFFFFFF, stroke=2)
splash.append(c)
```

## Key things you can do
| What you want | How to do it |
|---------------|--------------|
| Draw a rectangle | `Rect(x, y, width, height, fill=color, outline=color, stroke=1)` |
| Draw a circle | `Circle(x, y, radius, fill=color, outline=color)` |
| Draw a triangle | `Triangle(x0,y0, x1,y1, x2,y2, fill=color, outline=color)` |
| Draw a rounded rectangle | `RoundRect(x, y, width, height, radius, fill=color, outline=color)` |
| Draw a line | `Line(x0, y0, x1, y1, color)` |
| Draw a polygon | `Polygon(points=[(x0,y0),(x1,y1),...], outline=color)` |
| Transparent fill | Pass `fill=None` |
| Transparent outline | Pass `outline=None` |
| Change border thickness | `stroke=N` (integer, pixels) |
| Move a shape | Set `shape.x` and `shape.y` |

## Reading the official docs
[https://docs.circuitpython.org/projects/display-shapes/en/latest/](https://docs.circuitpython.org/projects/display-shapes/en/latest/)

The API reference lists every constructor parameter for each shape class. Pay attention to coordinate conventions: for `Rect`, `x` and `y` are the top-left corner; for `Circle`, they are the center. The `Polygon` class accepts any list of `(x, y)` tuples and is useful for arrows, stars, and other irregular outlines.

## Projects using this library
- [CircuitPython Display Support Using displayio](https://learn.adafruit.com/circuitpython-display-support-using-displayio) — The main displayio guide; includes examples of composing shapes and text into display scenes. *Credit: Adafruit Learning System*
- TFT graphics project in this wiki — Uses display shapes to build a simple graphical UI on a color TFT display.
