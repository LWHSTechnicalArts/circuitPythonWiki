# adafruit_mcp230xx

!!! info "Works with"
    Any board with I2C — Feather, Metro, Trinket M0, Raspberry Pi Pico, and more.

## What it does

The MCP23008 and MCP23017 are GPIO expander chips that give you extra digital I/O pins over I2C. If your board doesn't have enough pins to drive all your LEDs or read all your buttons, this is the fix.

- **MCP23008** — adds 8 digital I/O pins
- **MCP23017** — adds 16 digital I/O pins (two 8-pin banks: GPA and GPB)

Pins from the expander behave like regular `digitalio` pins, so code you already know works here.

Up to 8 MCP23017 chips can share one I2C bus by setting their three address pins (A0, A1, A2) to different combinations — that's up to 128 extra GPIO pins from a single I2C bus.

## Installing the library

Copy these to the `lib/` folder on your CIRCUITPY drive:

```
adafruit_mcp230xx/
adafruit_bus_device/
```

Both are in the [Adafruit CircuitPython Bundle](https://circuitpython.org/libraries).

## Quick start

```python
import board
import busio
import digitalio
from adafruit_mcp230xx.mcp23017 import MCP23017

i2c = busio.I2C(board.SCL, board.SDA)
mcp = MCP23017(i2c)  # default address 0x20

# Get a pin and use it like a regular digitalio pin
pin0 = mcp.get_pin(0)
pin0.direction = digitalio.Direction.OUTPUT
pin0.value = True   # turn it on

pin1 = mcp.get_pin(8)
pin1.direction = digitalio.Direction.INPUT
pin1.pull = digitalio.Pull.UP
print(pin1.value)
```

For the MCP23008, import `MCP23008` instead and use pins 0–7.

## Key things you can do

| What you want | How to do it |
|---|---|
| Get a pin object | `pin = mcp.get_pin(n)` where n is 0–15 for MCP23017 |
| Set a pin as output | `pin.direction = digitalio.Direction.OUTPUT` |
| Set a pin as input | `pin.direction = digitalio.Direction.INPUT` |
| Enable pull-up resistor | `pin.pull = digitalio.Pull.UP` |
| Write a pin high | `pin.value = True` |
| Read a pin | `print(pin.value)` |
| Use a second chip | `mcp2 = MCP23017(i2c, address=0x21)` |
| Write all pins at once | `mcp.gpio = 0xFF` (sets all GPA pins high) |

## Reading the official docs

Full API reference, including bulk pin reads and interrupt support:
[https://docs.circuitpython.org/projects/mcp230xx/en/latest/](https://docs.circuitpython.org/projects/mcp230xx/en/latest/)

## Projects using this library

- [Using MCP23008 and MCP23017 with CircuitPython](https://learn.adafruit.com/using-mcp23008-mcp23017-with-circuitpython) — Adafruit's full guide with wiring diagrams and example code
