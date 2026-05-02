# Motors

Motors let you affect the physical world in ways that light and sound alone cannot. A spinning wheel moves a robot. A servo arm opens a latch. A stepper motor positions a camera slider with sub-millimeter accuracy. A vibration motor gives a user tactile feedback without making a sound. Once you add motors to a project, it stops being a gadget and starts being a machine.

## Types of motors

**Servos** — A servo is a motor with a built-in gear box and position sensor. You send it an angle and it goes there. Standard hobby servos rotate 0–180 degrees. Continuous-rotation servos spin like a DC motor but are controlled the same way. Servos are the easiest motor to start with.

**DC motors** — A basic DC motor spins when you apply voltage. Reverse the polarity and it spins the other way. Speed is controlled by PWM (pulsing the power on and off rapidly). DC motors are cheap, powerful, and everywhere — fans, wheels, pumps. They require a driver chip to handle the current your board cannot supply.

**Stepper motors** — A stepper motor has multiple electromagnet coils that fire in sequence. Each "step" rotates the shaft by a fixed, repeatable angle — typically 1.8 degrees (200 steps per revolution). This makes them ideal for anything that needs precise positioning: 3D printers, CNC machines, camera sliders, scientific instruments. They are more complex to drive but uniquely reliable for motion control.

**Haptic motors** — A haptic motor is a small vibration motor (either ERM or LRA type) driven by a controller chip. The DRV2605L breakout gives you 123 built-in waveform effects. Haptic feedback adds a physical dimension to user interfaces — a confirmation click, an error rumble, an alert pulse.

## Power warning

Nearly every motor project has the same gotcha: **the board's 3.3V pin cannot drive motors directly.** Motors draw far more current than a microcontroller pin can safely supply, and many motors need 5V or higher. Trying to power a motor from a GPIO pin risks damaging the board.

You need either:

- An **external power supply** connected to a motor driver board (common for DC motors, steppers, and high-torque servos)
- A **FeatherWing or shield** like the Crickit or Motor Shield, which handles power routing for you
- A **dedicated driver breakout** like the A4988 for steppers

Always share a common ground between the board and the external motor power supply.

## Progression

| Level | Project | What you learn |
|---|---|---|
| Starter | [Servo Sweep](starter-servo-sweep.md) | PWM, servo angle control |
| Builder | [Motor Shield — DC Motors and Multiple Servos](builder-motor-shield.md) | MotorKit, I2C PWM expansion, DC throttle |
| Builder | [Robot with Crickit](builder-crickit-robot.md) | Crickit library, sensor-driven motion |
| Hacker | [Precision Stepper Motor Control](hacker-stepper-precision.md) | Step modes, current limiting, positioning |
| Hacker | [Haptic Feedback](hacker-haptic-feedback.md) | DRV2605L, waveform effects, sequencing |

Start with the servo sweep to get comfortable with the ideas. Move to DC motors when you want speed and power. Reach for steppers when position accuracy matters. Add haptics whenever your project has a user interface that benefits from physical response.
