# adafruit_fram

!!! info "Works with"
    Any board with I2C or SPI. Available in both I2C (up to 256 Kbit) and SPI (up to 4 Mbit) versions.

## What it does

`adafruit_fram` provides access to FRAM (ferroelectric RAM) non-volatile memory chips. FRAM is a compelling alternative to EEPROM and flash for storing small amounts of persistent data: it reads and writes at RAM speeds, retains data without power, and tolerates 10 trillion (10^13) write cycles — compared to around 100,000 for EEPROM and 10,000 to 100,000 for flash. This makes it well-suited for frequently-updated counters, configuration values, and small rolling logs where flash wear would eventually be a problem. The library exposes FRAM as a simple indexed list — you read and write individual bytes using array-style notation, making it feel like a persistent list.

## Installing the library

Copy all of the following to `CIRCUITPY/lib/`:

- `adafruit_fram.mpy`
- `adafruit_bus_device/` (folder)

## Quick start

```python
import board
import adafruit_fram

# I2C connection (most common breakout uses I2C)
i2c = board.I2C()
fram = adafruit_fram.FRAM_I2C(i2c)

# Read and write individual bytes
fram[0] = 42
print(fram[0])   # 42 — persists after power off

# Store a multi-byte value (manual byte packing)
import struct
value = 1234
packed = struct.pack(">H", value)   # big-endian unsigned short (2 bytes)
fram[0] = packed[0]
fram[1] = packed[1]

# Read it back
read_back = struct.unpack(">H", bytes([fram[0], fram[1]]))[0]
print(read_back)  # 1234

# Write a slice
fram[10:14] = b"test"
print(bytes(fram[10:14]))  # b'test'

# Read the full capacity
print(f"FRAM size: {len(fram)} bytes")
```

## Key things you can do

| What you want | How to do it |
|---------------|--------------|
| Write a byte | `fram[address] = value` (0–255) |
| Read a byte | `value = fram[address]` |
| Write multiple bytes at once | `fram[start:end] = b"data"` |
| Read multiple bytes at once | `bytes(fram[start:end])` |
| Store an integer > 255 | Use `struct.pack` to split into bytes |
| Check capacity | `len(fram)` — returns number of bytes |
| Use SPI FRAM instead | `adafruit_fram.FRAM_SPI(spi, cs)` |
| Use a non-default I2C address | `FRAM_I2C(i2c, address=0x51)` |
| Store config persistently | Write to known fixed addresses at startup, read them back |
| Increment a stored counter | `fram[0] = (fram[0] + 1) % 256` |

## Reading the official docs

Full API reference: [https://docs.circuitpython.org/projects/fram/en/latest/](https://docs.circuitpython.org/projects/fram/en/latest/)

## Projects using this library

- [Adafruit I2C Non-Volatile FRAM Breakout](https://learn.adafruit.com/adafruit-i2c-non-volatile-fram-breakout) — guide to the breakout board with wiring and read/write examples
