# USB Tricks

Your CircuitPython board is not just a USB drive. Plug it into a computer and it can
introduce itself as a keyboard, a mouse, a MIDI instrument, a gamepad, or several of
those at once. The operating system sees it as a real hardware device — no driver
installation required. This capability is called **USB HID**, short for Human Interface
Device, and it opens up a category of projects that most people assume requires
specialized hardware.

---

## What is USB HID?

When you plug a keyboard or mouse into a computer, the OS loads a generic HID driver
that speaks a standard protocol. Any device that speaks that same protocol gets recognized
automatically. CircuitPython boards with native USB can do exactly that — they enumerate
as HID devices right alongside your regular peripherals.

The library that makes this possible is `adafruit_hid`. It ships with pre-built classes
for the most common device types, and for advanced use you can define your own report
descriptor to create something entirely new.

---

## Which boards support this?

USB HID requires **native USB** — meaning the microcontroller itself handles USB, not a
separate chip. All of the following boards work:

| Board | USB HID | Notes |
|---|---|---|
| Trinket M0 | Yes | Small, affordable, great starter board |
| Feather M0 Express | Yes | Breadboard-friendly form factor |
| Feather M4 Express | Yes | Faster SAMD51, more RAM |
| Circuit Playground Express | Yes | Built-in buttons make wiring optional |
| Circuit Playground Bluefruit | Yes | Also supports BLE HID |
| ItsyBitsy M0 / M4 | Yes | Compact, strong pin count |
| RP2040 boards (Feather, KB2040, etc.) | Yes | Fast dual-core processor |
| Grand Central M4 | Yes | Many analog inputs — great for MIDI |

!!! warning "Boards that do NOT support USB HID"
    The ESP32-based boards (Feather ESP32-S2 and S3 are exceptions — they have native USB;
    the classic ESP32 does not). Metro M0/M4 and Arduino-style boards vary — check the
    board's CircuitPython page on circuitpython.org to confirm.

---

## What can you build?

### Keyboard and macro pads

Send any keystroke or key combination from a button press, a sensor reading, or a timer.
Common uses: shortcut launchers, accessibility devices, streaming control panels, prank
keyboards.

### Mouse control

Move the cursor, click buttons, and scroll — from a joystick, an accelerometer, a
distance sensor, or anything else you can read in CircuitPython.

### MIDI instruments

Your board appears as a USB MIDI device in GarageBand, Ableton, Logic, or any
DAW. Knobs become CC controllers, buttons become note triggers, sensors become
expressive performance tools.

### Gamepads and custom controllers

Define axes and buttons that map to a standard gamepad profile. Windows and macOS
recognize it natively. Works in Unity, Godot, and most game engines.

### Fully custom descriptors

At the hacker level, you write raw HID report descriptor bytes and define a device
that does not exist yet — 6 axes, 64 buttons, proprietary data formats. The OS
reads whatever you describe.

---

## Progression

These projects build on each other. Start at the level that matches where you are.

### Starter

[Your Board as a Keyboard](starter-hid-keyboard.md) — Wire up one button. Press it
and your board types a phrase. You will understand USB HID and the `adafruit_hid`
library by the end.

### Builder

[Customizing USB Devices](builder-customizing-usb.md) — Use `boot.py` to control
exactly what USB devices your board presents. Build a macropad with modifier keys,
media controls, and optional CIRCUITPY drive hiding.

[USB MIDI Controller](builder-midi-controller.md) — Turn potentiometers into MIDI CC
knobs. Any DAW sees your board as a USB MIDI device the moment you plug it in.

### Hacker

[Custom HID Device](hacker-custom-hid.md) — Write a raw HID report descriptor in
`boot.py` and create a device with axes, buttons, and behavior the pre-built classes
do not cover.

---

## How USB HID fits into the bigger picture

USB is just one transport. Many of these same ideas — sending keystrokes, controlling
a DAW, acting as a game controller — can also run wirelessly over Bluetooth LE on
boards that support it. Once you understand how HID works over USB, the BLE versions
will make immediate sense. See the [wireless section](../wireless/index.md) when you are ready
to cut the cord.
