# adafruit_seesaw

!!! info "Works with"
    Any board with I2C — Feather, Metro, Trinket M0, Raspberry Pi Pico, and more.

## What it does

Seesaw is Adafruit's co-processor protocol. Boards and breakouts that have a Seesaw chip (usually an ATSAMD09) expose digital I/O, analog inputs, PWM outputs, NeoPixel control, encoder reading, and more — all over a single I2C connection.

You don't talk to the hardware directly. You send commands to the Seesaw chip, and it handles the details.

**Products that use Seesaw:**

- NeoKey 1x4 — mechanical key switches with NeoPixels
- NeoTrellis — 4x4 grid of silicone buttons with NeoPixels
- STEMMA Soil Sensor — capacitive soil moisture + temperature
- Crickit — robotics add-on board for Circuit Playground

If a product lists "Seesaw" in its description, this is the library you need.

## Installing the library

Copy these to the `lib/` folder on your CIRCUITPY drive:

```
adafruit_seesaw/
adafruit_bus_device/
```

Both are in the [Adafruit CircuitPython Bundle](https://circuitpython.org/libraries).

## Quick start

```python
import board
import busio
from adafruit_seesaw.seesaw import Seesaw

i2c = busio.I2C(board.SCL, board.SDA)
ss = Seesaw(i2c)

# Digital input with pull-up
ss.pin_mode(2, ss.INPUT_PULLUP)
print(ss.digital_read(2))  # True = not pressed (pull-up)

# Digital output
ss.pin_mode(10, ss.OUTPUT)
ss.digital_write(10, True)

# Analog read
raw = ss.analog_read(2)  # returns 0–1023
print(raw)
```

Note: pin numbers refer to the Seesaw chip's internal pins, not your microcontroller's pins. Check the datasheet or Adafruit guide for the product you're using.

## Key things you can do

| What you want | How to do it |
|---|---|
| Set a pin as input (with pull-up) | `ss.pin_mode(n, ss.INPUT_PULLUP)` |
| Set a pin as input (no pull-up) | `ss.pin_mode(n, ss.INPUT)` |
| Set a pin as output | `ss.pin_mode(n, ss.OUTPUT)` |
| Read a digital pin | `ss.digital_read(n)` |
| Write a digital pin | `ss.digital_write(n, True)` |
| Read an analog pin | `ss.analog_read(n)` — returns 0–1023 |
| Set PWM duty cycle | `ss.analog_write(n, value)` — 0–255 |
| Read encoder position | `ss.encoder_position(n)` |
| Use a non-default address | `Seesaw(i2c, addr=0x37)` |

## Reading the official docs

Full API reference including NeoPixel control and encoder support:
[https://docs.circuitpython.org/projects/seesaw/en/latest/](https://docs.circuitpython.org/projects/seesaw/en/latest/)

## Projects using this library

- [Adafruit Seesaw ATSAMD09 Breakout](https://learn.adafruit.com/adafruit-seesaw-atsamd09-breakout) — introduction to the Seesaw protocol with wiring and code examples
- [Adafruit STEMMA Soil Sensor](https://learn.adafruit.com/adafruit-stemma-soil-sensor-ltr303-light-sensor) — capacitive soil moisture and temperature sensor built on Seesaw
