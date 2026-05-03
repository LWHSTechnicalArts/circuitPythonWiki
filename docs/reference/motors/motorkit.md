# adafruit_motorkit

!!! info "Works with"
    Any board with I2C. Designed for use with the Adafruit Motor Shield V2 (Arduino form factor) and the DC Motor + Stepper FeatherWing.

## What it does

`adafruit_motorkit` gives you high-level control of DC motors, stepper motors, and servos through the Adafruit Motor Shield V2 or the DC/Stepper Motor FeatherWing. Both boards use a PCA9685 PWM chip and TB6612 H-bridge drivers over I2C. Up to four DC motors or two stepper motors (or a combination) can be driven from a single board. No direct PWM pin management needed — just set throttle values or call `onestep()`.

## Installing the library

Copy all of the following to `CIRCUITPY/lib/`:

- `adafruit_motorkit.mpy`
- `adafruit_motor/` (folder)
- `adafruit_pca9685.mpy`
- `adafruit_bus_device/` (folder)

## Quick start

```python
import board
import time
from adafruit_motorkit import MotorKit
from adafruit_motor import stepper

i2c = board.I2C()
kit = MotorKit(i2c=i2c)

# DC motor on port M1
kit.motor1.throttle = 0.5   # 50% power forward
time.sleep(1)
kit.motor1.throttle = 0     # stop (brake)
kit.motor1.throttle = None  # coast

# Stepper motor on ports M3 + M4 (stepper1)
for _ in range(200):
    kit.stepper1.onestep(direction=stepper.FORWARD, style=stepper.SINGLE)
    time.sleep(0.01)

# Release stepper coils to save power
kit.stepper1.release()
```

## Key things you can do

| What you want | How to do it |
|---------------|--------------|
| Drive DC motor forward | `kit.motor1.throttle = 1.0` (full), `0.5` (half), etc. |
| Drive DC motor reverse | `kit.motor1.throttle = -0.75` |
| Brake a DC motor | `kit.motor1.throttle = 0` |
| Coast a DC motor | `kit.motor1.throttle = None` |
| Step a stepper motor | `kit.stepper1.onestep(direction=stepper.FORWARD, style=stepper.SINGLE)` |
| Use double-coil stepping | `style=stepper.DOUBLE` for more holding torque |
| Use interleaved stepping | `style=stepper.INTERLEAVE` for half-step resolution |
| Use microstepping | `style=stepper.MICROSTEP` for smooth, quiet motion |
| Step backward | `direction=stepper.BACKWARD` |
| Release stepper coils | `kit.stepper1.release()` — reduces heat and power draw when idle |
| Use two Motor Shield boards | `MotorKit(address=0x61)` for second board stacked with address jumper set |
| Access all four DC motors | `kit.motor1`, `kit.motor2`, `kit.motor3`, `kit.motor4` |
| Access both steppers | `kit.stepper1` (M1+M2), `kit.stepper2` (M3+M4) |

## Reading the official docs

Full API reference: [https://docs.circuitpython.org/projects/motorkit/en/latest/](https://docs.circuitpython.org/projects/motorkit/en/latest/)

## Projects using this library

- [Adafruit Motor Shield V2](https://learn.adafruit.com/adafruit-motor-shield-v2-for-arduino/python-circuitpython) — driving DC motors and steppers from a Metro or other Arduino-form-factor board
- [DC Motor + Stepper FeatherWing](https://learn.adafruit.com/adafruit-stepper-dc-motor-featherwing/circuitpython) — compact motor control stacked on a Feather board
