# adafruit_mcp3xxx

!!! info "Works with"
    Any board with SPI — Feather, Metro, Trinket M0, Raspberry Pi Pico, and more.

## What it does

The MCP3xxx family are SPI analog-to-digital converters (ADCs). They add multiple analog input channels to boards that have few or none.

- **MCP3008** — 8 single-ended analog input channels, 10-bit resolution
- **MCP3204** — 4 single-ended or 2 differential channels, 12-bit resolution
- **MCP3002** — 2 channels, 10-bit resolution

**Why you need this:** The Trinket M0 has only 3 analog pins. A Metro M0 has 6. An MCP3008 adds 8 more analog inputs using just 4 SPI wires (MOSI, MISO, SCK, CS) — and you can add multiple chips with separate CS pins.

Once connected, channels behave like `analogio.AnalogIn` objects with `.value` and `.voltage` properties.

## Installing the library

Copy these to the `lib/` folder on your CIRCUITPY drive:

```
adafruit_mcp3xxx/
adafruit_bus_device/
```

Both are in the [Adafruit CircuitPython Bundle](https://circuitpython.org/libraries).

## Quick start

```python
import board
import busio
import digitalio
from adafruit_mcp3xxx.mcp3008 import MCP3008
from adafruit_mcp3xxx.analog_in import AnalogIn

spi = busio.SPI(clock=board.SCK, MISO=board.MISO, MOSI=board.MOSI)
cs = digitalio.DigitalInOut(board.D5)

mcp = MCP3008(spi, cs)

# Read channel 0
chan0 = AnalogIn(mcp, MCP3008.P0)

print(chan0.value)    # integer 0–65535 (normalized from 10-bit)
print(chan0.voltage)  # float 0.0–3.3
```

All 8 channels (P0 through P7) work the same way.

## Key things you can do

| What you want | How to do it |
|---|---|
| Read a channel's raw value | `chan.value` — returns 0–65535 |
| Read a channel's voltage | `chan.voltage` — returns 0.0 to supply voltage |
| Access a different channel | `AnalogIn(mcp, MCP3008.P3)` — P0 through P7 |
| Add a second MCP3008 | Wire a second CS pin: `mcp2 = MCP3008(spi, cs2)` |
| Use MCP3204 instead | `from adafruit_mcp3xxx.mcp3204 import MCP3204` |
| Differential reading (MCP3204) | `AnalogIn(mcp, MCP3204.P0, MCP3204.P1)` |

## Reading the official docs

Full API reference, chip variants, and differential mode:
[https://docs.circuitpython.org/projects/mcp3xxx/en/latest/](https://docs.circuitpython.org/projects/mcp3xxx/en/latest/)

## Projects using this library

- [MCP3008 - 8-Channel 10-Bit ADC With SPI Interface](https://learn.adafruit.com/mcp3008-spi-adc) — Adafruit's full guide with wiring diagrams and potentiometer examples
