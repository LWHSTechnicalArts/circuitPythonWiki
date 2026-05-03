# SHT31-D

!!! info "Works with"
    Any CircuitPython board with I2C

## What it does
The SHT31-D measures both temperature and relative humidity in a single package. Accuracy is ±2% RH and ±0.3°C, making it one of the more precise combined sensors in this price range. Common applications include climate monitoring, data logging, HVAC control, and any project that needs both measurements without wiring two separate sensors. It also includes a built-in resistive heater that can be switched on briefly to burn off condensation — useful when operating in humid or dew-prone environments.

## Installing the library
Copy `adafruit_sht31d.mpy` from the Adafruit CircuitPython Bundle to your board's `lib/` folder. Also copy `adafruit_bus_device/` if it is not already present.

## Quick start
```python
import board
import busio
import adafruit_sht31d

i2c = busio.I2C(board.SCL, board.SDA)
sensor = adafruit_sht31d.SHT31D(i2c)

print(sensor.temperature)
print(sensor.relative_humidity)
```

## Key things you can do
| What you want | How to do it |
|---------------|--------------|
| Read temperature (°C) | `sensor.temperature` |
| Read relative humidity (%) | `sensor.relative_humidity` |
| Turn the heater on | `sensor.heater = True` |
| Turn the heater off | `sensor.heater = False` |

## Reading the official docs
[https://docs.circuitpython.org/projects/sht31d/en/latest/](https://docs.circuitpython.org/projects/sht31d/en/latest/)

The API reference covers `temperature`, `relative_humidity`, and `heater`. Use the heater sparingly — leaving it on continuously will skew temperature readings.

## Projects using this library
- [Adafruit SHT31-D Temperature and Humidity Sensor Breakout](https://learn.adafruit.com/adafruit-sht31-d-temperature-and-humidity-sensor-breakout) — wiring guide and CircuitPython examples. *Credit: Adafruit Learning System*
- [Air Quality Dashboard](../../projects/air-quality-dashboard.md) — combines temperature, humidity, and VOC data on a display. *Credit: This wiki*
