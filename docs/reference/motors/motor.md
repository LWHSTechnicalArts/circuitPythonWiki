# adafruit_motor

!!! info "Works with"
    Any board with PWM output (servos, DC motors) or digital GPIO pins (stepper motors). No I2C or SPI required.

## What it does

`adafruit_motor` provides low-level classes for controlling DC motors, standard and continuous-rotation servos, and stepper motors directly from CircuitPython. It operates at the signal level — you pass it PWM or digital pin objects and it handles the pulse math. This is the foundation that higher-level libraries like `adafruit_servokit` and `adafruit_motorkit` are built on.

## Installing the library

Copy the entire `adafruit_motor/` folder to your board's `CIRCUITPY/lib/` directory. The folder contains separate modules for each motor type.

## Quick start

```python
import board
import pwmio
from adafruit_motor import servo, motor, stepper

# Standard servo on pin D5
pwm = pwmio.PWMOut(board.D5, duty_cycle=0, frequency=50)
my_servo = servo.Servo(pwm)
my_servo.angle = 90  # move to center position

# Continuous rotation servo
pwm2 = pwmio.PWMOut(board.D6, duty_cycle=0, frequency=50)
cont_servo = servo.ContinuousServo(pwm2)
cont_servo.throttle = 0.5  # half speed forward

# DC motor (requires two PWM pins)
pwm_a = pwmio.PWMOut(board.D7, frequency=50)
pwm_b = pwmio.PWMOut(board.D8, frequency=50)
dc = motor.DCMotor(pwm_a, pwm_b)
dc.throttle = -1.0  # full speed reverse

# Stepper motor (four digital pins)
import digitalio
coils = (
    digitalio.DigitalInOut(board.D9),
    digitalio.DigitalInOut(board.D10),
    digitalio.DigitalInOut(board.D11),
    digitalio.DigitalInOut(board.D12),
)
for coil in coils:
    coil.direction = digitalio.Direction.OUTPUT

my_stepper = stepper.StepperMotor(*coils)
my_stepper.onestep(direction=stepper.FORWARD, style=stepper.SINGLE)
```

## Key things you can do

| What you want | How to do it |
|---------------|--------------|
| Move a servo to a position | `my_servo.angle = 0` through `180` |
| Set servo pulse range | `servo.Servo(pwm, min_pulse=500, max_pulse=2500)` |
| Spin a continuous servo | `cont_servo.throttle = 1.0` (full forward), `-1.0` (full reverse), `0` (stop) |
| Drive a DC motor forward | `dc.throttle = 0.8` (80% power) |
| Brake a DC motor | `dc.throttle = 0` |
| Coast a DC motor | `dc.throttle = None` |
| Step a stepper one step | `my_stepper.onestep(direction=stepper.FORWARD, style=stepper.SINGLE)` |
| Use microstepping | `style=stepper.MICROSTEP` for smoother, quieter motion |
| Step in double-coil mode | `style=stepper.DOUBLE` for more torque |
| Interleave steps | `style=stepper.INTERLEAVE` for half-step resolution |
| Reverse stepper direction | `direction=stepper.BACKWARD` |

## Reading the official docs

Full API reference: [https://docs.circuitpython.org/projects/motor/en/latest/](https://docs.circuitpython.org/projects/motor/en/latest/)

The docs cover all constructor parameters including `min_pulse`/`max_pulse` for servo calibration and the `actuation_range` property for mapping angles to custom degree ranges.

## Projects using this library

- [CircuitPython Servo](https://learn.adafruit.com/circuitpython-essentials/circuitpython-servo) — introductory servo control from the CircuitPython Essentials guide
- [Using Servos with CircuitPython](https://learn.adafruit.com/using-servos-with-circuitpython) — deeper dive into servo control, calibration, and multiple servo setups
