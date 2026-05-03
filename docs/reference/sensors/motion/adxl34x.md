# ADXL343 / ADXL345

!!! info "Works with"
    Any CircuitPython board with I2C or SPI

## What it does
The ADXL343 and ADXL345 are triple-axis accelerometers with a configurable range of ±2g to ±16g. They are older parts that appear frequently in existing projects and tutorials, making them worth knowing even though newer sensors like the LIS3DH offer similar capabilities. Like the LIS3DH, these sensors include hardware tap detection and freefall detection. The ADXL345 adds a higher output data rate option compared to the ADXL343. Both chips share the same CircuitPython library.

## Installing the library
Copy `adafruit_adxl34x.mpy` from the Adafruit CircuitPython Bundle to your board's `lib/` folder. Also copy `adafruit_bus_device/` if it is not already present.

## Quick start
```python
import board
import busio
import adafruit_adxl34x

i2c = busio.I2C(board.SCL, board.SDA)
sensor = adafruit_adxl34x.ADXL343(i2c)

x, y, z = sensor.acceleration
print(x, y, z)  # m/s²
```

## Key things you can do
| What you want | How to do it |
|---------------|--------------|
| Read acceleration (m/s²) | `sensor.acceleration` → `(x, y, z)` tuple |
| Enable tap detection | `sensor.enable_tap_detection(tap_count=1, threshold=20, duration=50, latency=20, window=255)` |
| Check for a tap event | `sensor.events["tap"]` |
| Enable freefall detection | `sensor.enable_freefall_detection(threshold=10, time=25)` |
| Check for freefall event | `sensor.events["freefall"]` |
| Set measurement range | `sensor.range = adafruit_adxl34x.Range.RANGE_4_G` |

## Reading the official docs
[https://docs.circuitpython.org/projects/adxl34x/en/latest/](https://docs.circuitpython.org/projects/adxl34x/en/latest/)

The `events` dictionary is the primary way to check tap and freefall status — reading a key clears the event flag, so each event is only reported once. The docs include default parameter values for `enable_tap_detection()` and `enable_freefall_detection()` so you can start with those and tune from there.

## Projects using this library
- [ADXL343 Breakout Learning Guide](https://learn.adafruit.com/adxl343-breakout-learning-guide) — wiring, event detection, and CircuitPython examples. *Credit: Adafruit Learning System*
