# adafruit_hid

!!! info "Works with"
    Any board with native USB. This includes SAMD21 (M0), SAMD51 (M4), RP2040, RP2350, ESP32-S2, and ESP32-S3 based boards. Boards that communicate over UART or Bluetooth only (such as ESP32 without native USB) cannot use this library.

## What it does
Makes your CircuitPython board appear to a connected computer as a standard USB HID (Human Interface Device) — a keyboard, mouse, consumer control (media keys), or gamepad. The host computer receives standard USB HID reports and needs no special drivers. This is the library behind DIY macro pads, foot pedals, shortcut keyboards, accessibility devices, and custom game controllers.

## Installing the library
Copy the `adafruit_hid/` folder from the bundle's `lib/` folder to your board's `lib/` folder.

## Quick start
```python
import time
import usb_hid
from adafruit_hid.keyboard import Keyboard
from adafruit_hid.keyboard_layout_us import KeyboardLayoutUS
from adafruit_hid.keycode import Keycode

keyboard = Keyboard(usb_hid.devices)
layout = KeyboardLayoutUS(keyboard)

time.sleep(1)  # brief delay on startup so the host enumerates the device

# Type a string
layout.write("Hello, world!\n")

# Press and release a key combination (Ctrl+C)
keyboard.press(Keycode.LEFT_CONTROL, Keycode.C)
keyboard.release_all()

# Move the mouse
from adafruit_hid.mouse import Mouse
mouse = Mouse(usb_hid.devices)
mouse.move(x=10, y=-5)       # move right 10px, up 5px
mouse.click(Mouse.LEFT_BUTTON)
```

## Key things you can do
| What you want | How to do it |
|---------------|--------------|
| Type text | `layout.write("string")` |
| Press a key | `keyboard.press(Keycode.A)` then `keyboard.release_all()` |
| Hold a modifier | `keyboard.press(Keycode.LEFT_SHIFT, Keycode.A)` |
| Release all keys | `keyboard.release_all()` |
| Move the mouse | `mouse.move(x=dx, y=dy, wheel=dw)` |
| Click mouse button | `mouse.click(Mouse.LEFT_BUTTON)` |
| Hold mouse button | `mouse.press(Mouse.RIGHT_BUTTON)` then `mouse.release(...)` |
| Media keys (play, volume) | `from adafruit_hid.consumer_control import ConsumerControl` + `ConsumerControlCode` |
| Custom gamepad | See the custom HID devices guide |

!!! warning "HID does not work in the REPL"
    The HID library only works when code runs from `code.py` on startup. Importing and using it interactively in the REPL will not produce HID output because the REPL uses the same USB connection for serial communication.

## Reading the official docs
[https://docs.circuitpython.org/projects/hid/en/latest/](https://docs.circuitpython.org/projects/hid/en/latest/)

The API reference documents every `Keycode` constant (hundreds of keys including function keys, numpad, and international keys), all `ConsumerControlCode` values (media play/pause, volume, brightness, etc.), and the `Mouse` button constants. Check there when you need a specific key that is not obvious — for example, `Keycode.WINDOWS` for the Super/Meta key or `Keycode.APPLICATION` for the context menu key.

## Projects using this library
- [CircuitPython Essentials: HID Keyboard and Mouse](https://learn.adafruit.com/circuitpython-essentials/circuitpython-hid-keyboard-and-mouse) — The foundational guide; covers keyboard, mouse, and consumer control with wiring and full code. *Credit: Adafruit Learning System*
- [Customizing USB Devices in CircuitPython](https://learn.adafruit.com/customizing-usb-devices-in-circuitpython/overview) — Shows how to change the USB device name, VID/PID, and combine multiple HID interfaces on one board. *Credit: Adafruit Learning System*
- [Custom HID Devices in CircuitPython](https://learn.adafruit.com/custom-hid-devices-in-circuitpython/overview) — Advanced guide for defining a custom HID descriptor — used to create gamepads and other non-standard HID devices. *Credit: Adafruit Learning System*
