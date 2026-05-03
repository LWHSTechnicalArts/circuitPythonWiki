# LIS3DH

!!! info "Works with"
    Any CircuitPython board with I2C or SPI

## What it does
The LIS3DH is a triple-axis accelerometer that measures acceleration on the X, Y, and Z axes. The measurement range is configurable from ±2g to ±16g. Beyond raw acceleration, it includes hardware tap detection (single and double tap) and freefall detection via interrupt pins, which allows the microcontroller to be woken or notified without polling the sensor continuously. Common applications include orientation sensing, step counting, impact detection, tilt-based controls, and wearable projects.

## Installing the library
Copy `adafruit_lis3dh.mpy` from the Adafruit CircuitPython Bundle to your board's `lib/` folder. Also copy `adafruit_bus_device/` if it is not already present.

## Quick start
```python
import board
import busio
import adafruit_lis3dh

i2c = busio.I2C(board.SCL, board.SDA)
sensor = adafruit_lis3dh.LIS3DH_I2C(i2c)

x, y, z = sensor.acceleration
print(x, y, z)  # m/s²
```

## Key things you can do
| What you want | How to do it |
|---------------|--------------|
| Read acceleration (m/s²) | `sensor.acceleration` → `(x, y, z)` tuple |
| Set measurement range | `sensor.range = adafruit_lis3dh.RANGE_4_G` (2, 4, 8, or 16 G) |
| Enable single tap detection | `sensor.set_tap(1, 80)` |
| Check if a tap occurred | `sensor.tapped` |
| Enable double tap detection | `sensor.set_tap(2, 80)` |

## Reading the official docs
[https://docs.circuitpython.org/projects/lis3dh/en/latest/](https://docs.circuitpython.org/projects/lis3dh/en/latest/)

The `set_tap()` arguments are tap count (1 or 2) and threshold (0–255, higher = less sensitive). The docs include a table of threshold values to help tune sensitivity. For interrupt-driven tap detection, wire the INT pin and see the interrupt examples in the guide.

## Projects using this library
- [Adafruit LIS3DH Triple-Axis Accelerometer Breakout](https://learn.adafruit.com/adafruit-lis3dh-triple-axis-accelerometer-breakout) — wiring, range settings, and tap detection walkthrough. *Credit: Adafruit Learning System*
- [Motion Alarm](../../projects/motion-alarm.md) — triggers an alert when the sensor detects movement or freefall. *Credit: This wiki*
- [Reactive Wearable](../../projects/reactive-wearable.md) — changes NeoPixel colors based on acceleration magnitude. *Credit: This wiki*
