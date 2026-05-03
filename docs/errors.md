# Common Errors (and What They Mean)

These are the errors beginners hit most often. Each one looks alarming the first time and obvious in hindsight. Read through them once so you recognize them when they show up.

---

## `ImportError: no module named 'adafruit_neopixel'`

**What it means:** The library file isn't on the board. CircuitPython can only import code that's physically stored in the `lib/` folder on your CIRCUITPY drive.

**How to fix it:**

1. Download the [Adafruit CircuitPython Bundle](https://circuitpython.org/libraries) that matches your CircuitPython version.
2. Find the file or folder for the library you need (e.g., `neopixel.mpy`).
3. Copy it into the `lib/` folder on your CIRCUITPY drive.
4. Save your code again — the board will restart and retry the import.

The error will name the missing module exactly. Whatever it says, that's the file you need to find in the bundle.

---

## `ValueError: Pin in use`

**What it means:** A pin is being claimed by two objects at the same time, or an object from a previous run was never cleaned up. CircuitPython keeps pin allocations alive across soft resets (when you save a file), so if your code crashes halfway through setup, that pin stays claimed.

**How to fix it:**

- Press the RESET button (or unplug and replug) for a full hard reset, which clears all pin allocations.
- Or add this at the very top of your `code.py` for development:

```python
import microcontroller
microcontroller.reset()
```

Remove it when you're done debugging.

- Check that you're not creating two objects using the same pin in the same run.

---

## `IndentationError: unexpected indent` / `IndentationError: expected an indented block`

**What it means:** Python uses whitespace to define code structure, and something is off. Either a line is indented when it shouldn't be, or a block that requires indented code (like after `if:` or `def:`) has none.

**How to fix it:**

- Check the line number in the error message.
- Make sure you're using spaces consistently — don't mix tabs and spaces. Use 4 spaces per indent level throughout.
- Look for a missing colon (`:`) at the end of an `if`, `for`, `while`, `def`, or `class` line.
- Most code editors can show whitespace characters — turn that on if you're confused.

Common pattern that triggers `expected an indented block`:

```python
def my_function():
    # forgot to write the body
```

Add a `pass` statement if the function body is intentionally empty for now.

---

## `OSError: [Errno 19] No such device`

**What it means:** Your code tried to communicate with an I2C device, but nothing responded at that address. The device isn't being detected on the bus.

**How to fix it:**

1. **Check wiring** — SDA goes to SDA, SCL goes to SCL. These are easy to swap.
2. **Check power** — the sensor needs 3.3V (or 5V if specified). Check that VIN and GND are connected.
3. **Check pull-ups** — I2C requires pull-up resistors on both SDA and SCL. Most breakout boards include them; if you're wiring a bare chip, add 4.7kΩ resistors to 3.3V.
4. **Scan the bus** to find what addresses are actually responding:

```python
import board
import busio

i2c = busio.I2C(board.SCL, board.SDA)
while not i2c.try_lock():
    pass
print(i2c.scan())
i2c.unlock()
```

This prints a list of addresses (as integers). If it returns `[]`, nothing is found — go back to wiring. If it returns an address you don't expect, check the device's address configuration.

---

## `RuntimeError: Drive is read-only`

**What it means:** The CIRCUITPY drive is mounted in read-only mode. This usually happens when the drive is also mounted on your computer — some operating systems (especially Linux) mount it in a way that locks CircuitPython out of writing to it.

**How to fix it:**

- **On any OS:** Safely eject the CIRCUITPY drive from your computer, then unplug and replug the board. It should remount normally.
- **On Linux:** This is especially common. Eject the drive from the file manager before trying to save.
- Check that your code isn't trying to write to the filesystem while it's mounted on a computer. CircuitPython can write to its own drive, but only when the computer isn't also mounting it in a conflicting mode.

---

## `MemoryError: memory allocation failed`

**What it means:** The board ran out of RAM. CircuitPython boards have very limited memory — a Trinket M0 has 32KB, a Feather M4 has 192KB. Large images, long strings, or many loaded libraries can fill it up.

**How to fix it:**

- **Load less.** Every library you import takes RAM. Only import what you need.
- **Use smaller fonts.** A large `.bdf` font file can use 30KB by itself. Use `terminalio.FONT` or a small point size.
- **Avoid large string concatenation.** Building a big string character by character is expensive. Use `join()` or write to a buffer.
- **Free unused objects.** Call `gc.collect()` after deleting large objects:

```python
import gc
del big_image
gc.collect()
print(gc.mem_free())  # check how much RAM you recovered
```

- **Use a board with more RAM** for memory-intensive projects — Feather M4, Metro M4, or Raspberry Pi Pico all have significantly more.

---

## `AttributeError: 'module' object has no attribute '...'`

**What it means:** You're trying to call a function or access a property that doesn't exist in the version of the library installed on your board. This usually means a version mismatch between your library bundle and your CircuitPython version.

**How to fix it:**

1. Check your CircuitPython version: it's printed in the REPL when you connect, or in `boot_out.txt` on the CIRCUITPY drive.
2. Download the library bundle that matches that version from [circuitpython.org/libraries](https://circuitpython.org/libraries).
3. Replace the library file or folder in `lib/` with the version from the matching bundle.
4. Also check that you're importing from the right module — a typo in the import path can produce this error.

---

## `SyntaxError: invalid syntax`

**What it means:** Python can't parse your code because of a typo or structural mistake. The line it points to has something Python doesn't understand.

**How to fix it:**

- Look at the exact line number in the error.
- Common causes:
    - Missing closing parenthesis, bracket, or quote from a previous line
    - A reserved word used as a variable name (e.g., `list = []`)
    - An `=` where `==` was needed inside an `if` condition
    - Python 2 syntax in Python 3 code (e.g., `print "hello"` instead of `print("hello")`)

The error sometimes points to the line *after* the actual problem, because Python only realizes something is wrong when it hits an unexpected token.

---

## The board isn't showing up as a USB drive

**What it means:** The board isn't mounting as CIRCUITPY. This can happen after a crash, a bad `.uf2` install, a filesystem error, or just a bad cable.

**How to fix it:**

1. **Double-tap the RESET button** quickly. The onboard LED should start doing a slow rainbow chase or pulsing green — this means you're in the bootloader (UF2 mode).
2. A drive called `CIRCUITBOOT` or similar should now appear on your computer.
3. Drag a fresh CircuitPython `.uf2` file onto that drive. Download the correct one for your board from [circuitpython.org/downloads](https://circuitpython.org/downloads).
4. The board will restart and CIRCUITPY will reappear.

If double-tap doesn't work, try a slower or faster tap rhythm — it varies slightly between boards.

!!! warning "Check your cable"
    Many USB cables are charge-only and don't carry data. If the board never shows up at all (not even in bootloader mode), try a different cable first.

---

## Code runs once but doesn't loop

**What it means:** Your code runs top to bottom and then stops. There's no `while True:` loop keeping it running, or the loop exists but isn't structured correctly.

**How to fix it:**

Make sure your main logic is inside a `while True:` block, and that it's correctly indented:

```python
import board
import time

# Setup goes here, outside the loop
led = digitalio.DigitalInOut(board.LED)
led.direction = digitalio.Direction.OUTPUT

# Main loop — runs forever
while True:
    led.value = True
    time.sleep(0.5)
    led.value = False
    time.sleep(0.5)
```

If your `while True:` is there but nothing after it runs, check the indentation. Everything inside the loop must be indented one level relative to the `while`.

---

## Still stuck?

When in doubt, check the Adafruit Discord #circuitpython channel — it's friendly and usually fast.
