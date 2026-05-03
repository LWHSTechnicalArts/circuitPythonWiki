# VL53L0X

!!! info "Works with"
    Any CircuitPython board with I2C

## What it does

The VL53L0X uses an invisible 940nm laser and time-of-flight measurement to determine distance. Unlike ultrasonic sensors, it is unaffected by surface texture, color, or angle within its cone. Effective range is 50mm to 1200mm (about 5cm to 120cm) with roughly ±3% accuracy. The I2C interface makes wiring simple, and multiple sensors can share one bus by reassigning addresses at startup.

## Installing the library

Download `adafruit_vl53l0x.mpy` from the [Adafruit CircuitPython Bundle](https://circuitpython.org/libraries) and copy it to the `lib/` folder on your board.

## Quick start

```python
import time
import board
import busio
import adafruit_vl53l0x

i2c = busio.I2C(board.SCL, board.SDA)
sensor = adafruit_vl53l0x.VL53L0X(i2c)

while True:
    print(f"Distance: {sensor.range} mm")
    time.sleep(0.1)
```

## Key things you can do

| What you want | How to do it |
|---------------|--------------|
| Read distance | `sensor.range` — returns millimeters as an integer |
| Improve accuracy (slower reads) | `sensor.measurement_timing_budget = 200000` — microseconds; default is 33000 |
| Speed up reads (less accurate) | `sensor.measurement_timing_budget = 20000` |
| Change I2C address | `sensor.set_address(0x30)` — use before releasing XSHUT pin when daisy-chaining |
| Use multiple sensors on one bus | Hold each XSHUT low, initialize one at a time, call `set_address()` on each |

## Reading the official docs

Full API reference: [https://docs.circuitpython.org/projects/vl53l0x/en/latest/](https://docs.circuitpython.org/projects/vl53l0x/en/latest/)

Key things to look up:

- `measurement_timing_budget` is in microseconds — higher values improve accuracy by averaging more samples internally; 200000 (200ms) is a good balance for slow-moving targets
- The default I2C address is `0x29`; use XSHUT pins to sequence multiple sensors during address reassignment
- Readings beyond ~1200mm or with the sensor pointed at a non-reflective black surface may return `8190` or `8191` — treat those as out-of-range sentinels

## Projects using this library

- [Adafruit VL53L0X guide](https://learn.adafruit.com/adafruit-vl53l0x-micro-lidar-distance-sensor-breakout/python-circuitpython) — wiring, address changing for multiple sensors, and accuracy tuning
