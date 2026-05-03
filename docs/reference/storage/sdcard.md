# adafruit_sdcard

!!! info "Works with"
    Any board with SPI. Uses the `storage` module built into CircuitPython — no extra dependency beyond the library itself.

## What it does

`adafruit_sdcard` mounts a FAT-formatted SD card as a filesystem in CircuitPython. Once mounted, you can read and write files using standard Python file I/O — `open()`, `read()`, `write()`, `os.listdir()`, and so on. This is the standard way to log sensor data to a file, store audio samples, cache large assets, or work with data files too large to fit in the board's internal flash storage. SD cards are also easy to transfer to a computer for analysis without a USB connection.

The `storage` module that handles mounting is built into CircuitPython and does not need to be installed separately.

## Installing the library

Copy the following to `CIRCUITPY/lib/`:

- `adafruit_sdcard.mpy`

## Quick start

```python
import board
import busio
import digitalio
import storage
import adafruit_sdcard

# SPI bus and chip select
spi = busio.SPI(board.SCK, board.MOSI, board.MISO)
cs = digitalio.DigitalInOut(board.D10)

# Mount the SD card
sdcard = adafruit_sdcard.SDCard(spi, cs)
vfs = storage.VfsFat(sdcard)
storage.mount(vfs, "/sd")
print("SD card mounted!")

# Write to a file
with open("/sd/data.txt", "w") as f:
    f.write("temperature,humidity\n")
    f.write("22.5,45.3\n")

# Append to a file (useful for logging)
with open("/sd/data.txt", "a") as f:
    f.write("23.1,44.8\n")

# Read a file
with open("/sd/data.txt", "r") as f:
    print(f.read())

# List files on the card
import os
print(os.listdir("/sd"))

# Unmount cleanly before removing the card
storage.umount("/sd")
```

## Key things you can do

| What you want | How to do it |
|---------------|--------------|
| Mount the SD card | `storage.mount(storage.VfsFat(sdcard), "/sd")` |
| Write a new file | `open("/sd/filename.txt", "w")` |
| Append to an existing file | `open("/sd/filename.txt", "a")` — use for data logging |
| Read a file | `open("/sd/filename.txt", "r")` |
| List files | `os.listdir("/sd")` |
| Check free space | `os.statvfs("/sd")` |
| Create a directory | `os.mkdir("/sd/mydir")` |
| Delete a file | `os.remove("/sd/filename.txt")` |
| Unmount before card removal | `storage.umount("/sd")` — prevents corruption |
| Use CSV for sensor logging | Write comma-separated lines with headers for easy import into spreadsheets |

## Reading the official docs

Full API reference: [https://docs.circuitpython.org/projects/sd/en/latest/](https://docs.circuitpython.org/projects/sd/en/latest/)

## Projects using this library

- [Adafruit Micro SD Breakout Board](https://learn.adafruit.com/adafruit-micro-sd-breakout-board-card-tutorial/circuitpython) — wiring guide and code examples for the Adafruit SD card breakout
