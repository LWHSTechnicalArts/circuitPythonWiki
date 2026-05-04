# SGP40

!!! info "Works with"
    Any CircuitPython board with I2C

## What it does
The SGP40 is a metal-oxide (MOX) VOC (volatile organic compound) sensor that outputs a VOC index from 0 to 500. Higher values indicate worse air quality — 100 is the baseline for normal indoor air, and readings above 250 suggest elevated contamination from sources like cleaning products, cooking, or off-gassing materials. Because MOX sensors are temperature and humidity dependent, the SGP40 requires compensation values from a paired temperature/humidity sensor (such as an SHT31-D or SHT4x) to produce accurate results. Allow at least 10 seconds after startup before treating readings as stable.

## Installing the library
Copy the `adafruit_sgp40/` folder from the Adafruit CircuitPython Bundle to your board's `lib/` folder. Also copy `adafruit_bus_device/` if it is not already present. For the compensation calculation to work you will also need a temperature/humidity sensor library (e.g. `adafruit_sht31d.mpy`).

## Quick start
```python
import board
import busio
import adafruit_sgp40
import adafruit_sht31d

i2c = busio.I2C(board.SCL, board.SDA)
sht = adafruit_sht31d.SHT31D(i2c)
sgp = adafruit_sgp40.SGP40(i2c)

temperature = sht.temperature
humidity = sht.relative_humidity

voc_index = sgp.measure_index(temperature=temperature, relative_humidity=humidity)
print(voc_index)
```

## Key things you can do
| What you want | How to do it |
|---------------|--------------|
| Read VOC index (0-500) | `sgp.measure_index(temperature=temp, relative_humidity=rh)` |
| Read without compensation | `sgp.measure_raw()` (raw sensor counts, not recommended for display) |
| Wait for stable readings | Allow 10 seconds after power-on before logging values |

## Reading the official docs
[https://docs.circuitpython.org/projects/sgp40/en/latest/](https://docs.circuitpython.org/projects/sgp40/en/latest/)

The key method is `measure_index()`. The docs explain the VOC algorithm internals — the algorithm keeps a running baseline, so the index improves in accuracy over the first few minutes of operation.

## Projects using this library
- [Adafruit SGP40 Air Quality Sensor](https://learn.adafruit.com/adafruit-sgp40) — hardware overview and CircuitPython example with SHT31 compensation. *Credit: Adafruit Learning System*
- [Air Quality Dashboard](../../../projects/sensors/hacker-air-quality-dash.md) — displays VOC index alongside temperature and humidity. *Credit: This wiki*
