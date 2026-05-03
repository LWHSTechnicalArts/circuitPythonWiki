# adafruit_servokit

!!! info "Works with"
    Any board with I2C. The PCA9685 PWM driver handles all the PWM output — your board only needs two pins (SDA and SCL).

## What it does

`adafruit_servokit` provides high-level control of up to 16 servos (or a mix of standard and continuous-rotation servos) through the PCA9685 16-channel PWM driver chip over I2C. Instead of managing PWM pins directly, you index into `kit.servo[]` or `kit.continuous_servo[]` and set angles or throttle values. Boards using this chip include the Adafruit 16-Channel PWM/Servo Shield, the 8-Channel Servo FeatherWing, and many third-party driver boards.

## Installing the library

Copy all of the following to `CIRCUITPY/lib/`:

- `adafruit_servokit.mpy`
- `adafruit_pca9685.mpy`
- `adafruit_motor/` (folder)
- `adafruit_bus_device/` (folder)

## Quick start

```python
import board
from adafruit_servokit import ServoKit

i2c = board.I2C()
kit = ServoKit(channels=16)  # use channels=8 for 8-channel boards

# Standard servo on channel 0
kit.servo[0].angle = 0    # move to 0 degrees
kit.servo[0].angle = 90   # move to center
kit.servo[0].angle = 180  # move to 180 degrees

# Continuous rotation servo on channel 1
kit.continuous_servo[1].throttle = 0.5   # half speed forward
kit.continuous_servo[1].throttle = -1.0  # full speed reverse
kit.continuous_servo[1].throttle = 0     # stop

# Calibrate a servo's pulse range if needed
kit.servo[2].set_pulse_width_range(500, 2500)
```

## Key things you can do

| What you want | How to do it |
|---------------|--------------|
| Move a servo to an angle | `kit.servo[n].angle = 90` (0–180) |
| Read current servo angle | `print(kit.servo[n].angle)` |
| Spin a continuous servo | `kit.continuous_servo[n].throttle = 1.0` (forward), `-1.0` (reverse) |
| Stop a continuous servo | `kit.continuous_servo[n].throttle = 0` |
| Calibrate pulse width | `kit.servo[n].set_pulse_width_range(min_us, max_us)` |
| Change actuation range | `kit.servo[n].actuation_range = 270` for wide-range servos |
| Use a non-default I2C address | `ServoKit(channels=16, address=0x41)` |
| Stack multiple PCA9685 boards | Use different I2C addresses (0x40–0x7F), create multiple `ServoKit` instances |
| Set PWM frequency | `kit.frequency = 60` (default is 50 Hz for servos) |

## Reading the official docs

Full API reference: [https://docs.circuitpython.org/projects/servokit/en/latest/](https://docs.circuitpython.org/projects/servokit/en/latest/)

## Projects using this library

- [16-Channel PWM/Servo Shield](https://learn.adafruit.com/adafruit-16-channel-pwm-slash-servo-shield/circuitpython-usage) — using the Arduino shield form-factor board with CircuitPython
- [8-Channel Servo FeatherWing](https://learn.adafruit.com/adafruit-8-channel-pwm-or-servo-featherwing/circuitpython) — compact servo control stacked on a Feather board
