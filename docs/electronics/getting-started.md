# Getting Started

!!! info "Works with"
    Any CircuitPython board

This page will get you from an out-of-the-box board to a running program. Follow the steps in order the first time — each one builds on the last.

---

## What You Need

- A CircuitPython-compatible board (Trinket M0, Feather, Circuit Playground, Pico W, or similar)
- A USB cable that fits your board and carries data (not just power — some cheap cables are charge-only)
- A computer running macOS, Windows, or Linux

That is it. No breadboard, no extra components, and no software installation is strictly required to get started.

---

## Installing CircuitPython

Most boards ship with CircuitPython already installed. If yours does not, or if you want to update to a newer version:

1. Go to [circuitpython.org/downloads](https://circuitpython.org/downloads) and find your board.
2. Download the `.uf2` file for the latest stable release.
3. Put your board into bootloader mode. On most boards this means double-pressing the reset button — the board will appear on your computer as a USB drive named something like `TRINKETBOOT` or `RPI-RP2`.
4. Drag the `.uf2` file onto that drive. The board will reboot automatically and reappear as a drive named `CIRCUITPY`.

Once you see `CIRCUITPY`, CircuitPython is installed and ready.

!!! note
    The exact bootloader steps vary slightly by board. If a double-press does not work, check the Adafruit guide for your specific board at [learn.adafruit.com](https://learn.adafruit.com).

---

## Choosing an Editor

You write CircuitPython code in plain text files, so almost any editor will work. Here are three good options:

- **Mu Editor** ([codewith.mu](https://codewith.mu/)) — Recommended for beginners; it detects your CircuitPython board automatically and includes a serial console for seeing output and errors.
- **Thonny** ([thonny.org](https://thonny.org/)) — A solid alternative that will feel familiar if you have already used it for Python class.
- **code.circuitpython.org** — A browser-based editor with no installation required; it connects to your board over USB or Bluetooth directly from Chrome or Edge.

These editors are listed in the [Awesome CircuitPython](https://github.com/adafruit/awesome-circuitpython) community resource list maintained by Adafruit and the CircuitPython community.

---

## Your First Program

The `CIRCUITPY` drive contains a file called `code.py`. Whatever is in that file runs when the board powers up. Open it in your editor, replace its contents with the following, and save:

```python
import board
import digitalio
import time

led = digitalio.DigitalInOut(board.LED)
led.direction = digitalio.Direction.OUTPUT

while True:
    led.value = True
    time.sleep(0.5)
    led.value = False
    time.sleep(0.5)
```

The built-in LED should start blinking immediately after you save. If it does not, open the serial console in your editor — any error message will appear there.

!!! tip
    On CircuitPython, saving the file is enough to restart the program. You never need to upload or flash anything — just save and watch it run.

---

## Installing Libraries

CircuitPython has a large collection of libraries for sensors, displays, wireless modules, and more. Libraries are distributed as `.mpy` files (pre-compiled for size and speed) and organized into a downloadable bundle.

To install a library:

1. Download the CircuitPython Library Bundle for your version of CircuitPython from [circuitpython.org/libraries](https://circuitpython.org/libraries). Make sure the bundle version matches your CircuitPython version (e.g., bundle 9.x for CircuitPython 9).
2. Unzip the bundle. Inside you will find a `lib/` folder containing hundreds of `.mpy` files and subfolders.
3. Find the file or folder for the library you need — for example, `neopixel.mpy` or the `adafruit_lis3dh/` folder.
4. Copy just that file or folder into the `lib/` folder on your `CIRCUITPY` drive.

Copy only what your project needs. Board storage is limited, and copying the entire bundle will likely fill it up.

!!! warning "Match your versions"
    Libraries from the wrong bundle version can cause subtle errors. Always download the bundle that matches the CircuitPython version shown at the top of the serial console when your board starts.

---

## Where to Get Help

Getting stuck is a normal part of learning. Here are the best places to get unstuck:

- **Adafruit Discord** — Join at [adafruit.com/discord](https://adafruit.com/discord) and head to the `#circuitpython` channel. The community is active and welcoming, and Adafruit staff monitor it regularly.
- **Adafruit Forums** — [forums.adafruit.com](https://forums.adafruit.com) is a searchable archive of questions and answers. Chances are someone has already run into the same problem.
