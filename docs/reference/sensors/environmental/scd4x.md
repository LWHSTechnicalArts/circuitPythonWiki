# SCD40 / SCD41

!!! info "Works with"
    Any CircuitPython board with I2C

## What it does
The SCD40 and SCD41 are true CO2 sensors that use photoacoustic NDIR (non-dispersive infrared) technology to measure CO2 concentration directly — not an estimate derived from VOC readings. They also measure temperature and relative humidity in the same package. Typical CO2 measurement range is 400 to 2000 ppm (SCD40) or 400 to 5000 ppm (SCD41), with ±50 ppm accuracy. These sensors are well suited for ventilation control, classroom air quality monitoring, and any application where knowing actual CO2 levels matters.

## Installing the library
Copy `adafruit_scd4x.mpy` from the Adafruit CircuitPython Bundle to your board's `lib/` folder. Also copy `adafruit_bus_device/` if it is not already present.

## Quick start
```python
import board
import busio
import adafruit_scd4x

i2c = busio.I2C(board.SCL, board.SDA)
scd4x = adafruit_scd4x.SCD4X(i2c)

scd4x.start_periodic_measurement()

while True:
    if scd4x.data_ready:
        print(scd4x.CO2)
        print(scd4x.temperature)
        print(scd4x.relative_humidity)
```

## Key things you can do
| What you want | How to do it |
|---------------|--------------|
| Start continuous measurements | `scd4x.start_periodic_measurement()` (call once at startup) |
| Check if new data is available | `scd4x.data_ready` |
| Read CO2 (ppm) | `scd4x.CO2` |
| Read temperature (°C) | `scd4x.temperature` |
| Read relative humidity (%) | `scd4x.relative_humidity` |
| Stop measurements | `scd4x.stop_periodic_measurement()` |

## Reading the official docs
[https://docs.circuitpython.org/projects/scd4x/en/latest/](https://docs.circuitpython.org/projects/scd4x/en/latest/)

The sensor updates every 5 seconds. You must call `start_periodic_measurement()` before any readings are available — the sensor does not start measuring automatically on power-on. The docs also cover single-shot measurement mode (lower power, one reading on demand) and factory calibration methods.

## Projects using this library
- [Adafruit SCD-40 and SCD-41](https://learn.adafruit.com/adafruit-scd-40-and-scd-41) — hardware overview, wiring, and CircuitPython walkthrough. *Credit: Adafruit Learning System*
