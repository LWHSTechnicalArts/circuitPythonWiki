# adafruit_bus_device

!!! info "Works with"
    Any CircuitPython board.

## What it does

`adafruit_bus_device` handles the low-level mechanics of I2C and SPI communication: bus locking, transaction retries, chip select toggling, and safe multi-device access. It is the foundation that nearly every sensor and display library in the Adafruit ecosystem is built on.

**You almost never use this library directly.** You install it because other libraries depend on it.

If you see this error:

```
ImportError: no module named 'adafruit_bus_device'
```

You just need to copy the folder to `lib/`.

## Installing the library

Copy the entire folder to the `lib/` folder on your CIRCUITPY drive:

```
adafruit_bus_device/
```

Available in the [Adafruit CircuitPython Bundle](https://circuitpython.org/libraries).

## Quick start

You rarely call `adafruit_bus_device` yourself. Here's what it looks like under the hood when a sensor library uses it — and what you'd write if you were building your own driver:

```python
import board
import busio
from adafruit_bus_device.i2c_device import I2CDevice

i2c = busio.I2C(board.SCL, board.SDA)
device = I2CDevice(i2c, 0x48)  # device at address 0x48

# Read 2 bytes from the device
with device:
    result = bytearray(2)
    device.readinto(result)

# Write a byte, then read a response
with device:
    device.write(bytes([0x01]))  # send register address
    result = bytearray(2)
    device.readinto(result)
```

The `with device:` block locks the I2C bus for the duration of the transaction and releases it automatically.

## Key things you can do

| What you want | How to do it |
|---|---|
| Create an I2C device handle | `I2CDevice(i2c, address)` |
| Create an SPI device handle | `SPIDevice(spi, cs, baudrate=100000)` |
| Read bytes from a device | `device.readinto(buf)` inside `with device:` |
| Write bytes to a device | `device.write(buf)` inside `with device:` |
| Write then read (common pattern) | `device.write_then_readinto(out_buf, in_buf)` |
| Lock the bus for a transaction | Use `with device:` — locks and unlocks automatically |

## Reading the official docs

Full API reference for both I2CDevice and SPIDevice:
[https://docs.circuitpython.org/projects/busdevice/en/latest/](https://docs.circuitpython.org/projects/busdevice/en/latest/)

!!! tip "Writing your own driver?"
    Use `I2CDevice` and `SPIDevice` instead of talking to the bus directly. They handle edge cases and make your code work cleanly alongside other libraries sharing the same bus.
