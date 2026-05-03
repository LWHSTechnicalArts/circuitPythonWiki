# VL53L1X

!!! info "Works with"
    Any CircuitPython board with I2C

## What it does

The VL53L1X is the extended-range version of ST's time-of-flight sensor family. It reaches up to 4 meters in long-distance mode and adds a region-of-interest (ROI) feature that lets you narrow the detection cone — useful for ignoring objects outside a specific zone. Like the VL53L0X, it uses an invisible 940nm laser and is unaffected by target color or surface texture.

## Installing the library

Download `adafruit_vl53l1x.mpy` from the [Adafruit CircuitPython Bundle](https://circuitpython.org/libraries) and copy it to the `lib/` folder on your board.

## Quick start

```python
import time
import board
import busio
import adafruit_vl53l1x

i2c = busio.I2C(board.SCL, board.SDA)
sensor = adafruit_vl53l1x.VL53L1X(i2c)

sensor.distance_mode = 2        # 1 = short (up to 1.3m), 2 = long (up to 4m)
sensor.timing_budget = 50       # milliseconds

sensor.start_ranging()

while True:
    if sensor.data_ready:
        distance = sensor.distance
        if distance is not None:
            print(f"Distance: {distance} cm")
        sensor.clear_interrupt()
    time.sleep(0.01)
```

## Key things you can do

| What you want | How to do it |
|---------------|--------------|
| Start ranging | `sensor.start_ranging()` |
| Stop ranging | `sensor.stop_ranging()` |
| Read distance | `sensor.distance` — returns cm as a float, or `None` if not ready |
| Check if data is available | `sensor.data_ready` — `True` when a new measurement is complete |
| Clear the data-ready flag | `sensor.clear_interrupt()` — must be called after each read |
| Set distance mode | `sensor.distance_mode = 1` (short, up to 1.3m) or `2` (long, up to 4m) |
| Set timing budget | `sensor.timing_budget = 50` — milliseconds; higher = more accurate, slower |

## Reading the official docs

Full API reference: [https://docs.circuitpython.org/projects/vl53l1x/en/latest/](https://docs.circuitpython.org/projects/vl53l1x/en/latest/)

Key things to look up:

- Unlike VL53L0X, the VL53L1X requires an explicit `start_ranging()` / `stop_ranging()` cycle; reading `sensor.distance` without starting will return `None`
- `clear_interrupt()` must be called after every read or `data_ready` will not go `True` again
- Short mode (1) is more immune to ambient light interference; long mode (2) reaches farther but is more affected by sunlight
- Timing budget options: 15, 20, 33, 50, 100, 200, 500 ms — 15ms is the minimum and only available in short mode

## Projects using this library

- [Adafruit VL53L1X guide](https://learn.adafruit.com/adafruit-vl53l1x/python-circuitpython) — wiring, distance mode comparison, and ROI configuration
