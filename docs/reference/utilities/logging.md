# adafruit_logging

!!! info "Works with"
    Any CircuitPython board.

## What it does

`adafruit_logging` brings Python-style logging to CircuitPython. Instead of scattering `print()` calls everywhere, you log messages at different severity levels. You can then control which messages actually appear by setting a minimum level — so during development you see everything, and in production you only see warnings and errors.

**Log levels, from least to most severe:**

| Level | When to use it |
|---|---|
| `DEBUG` | Detailed diagnostic info you only want during development |
| `INFO` | Normal progress messages ("connected", "starting up") |
| `WARNING` | Something unexpected happened but the code can continue |
| `ERROR` | A specific operation failed |
| `CRITICAL` | The program cannot continue |

## Installing the library

Copy this to the `lib/` folder on your CIRCUITPY drive:

```
adafruit_logging.mpy
```

Available in the [Adafruit CircuitPython Bundle](https://circuitpython.org/libraries).

## Quick start

```python
import adafruit_logging as logging

logger = logging.getLogger("my_project")
logger.setLevel(logging.INFO)

logger.debug("This won't show — below INFO level")
logger.info("Starting up")
logger.warning("Sensor returned unexpected value")
logger.error("Could not connect to network")
logger.critical("Out of memory, halting")
```

Output in the REPL:

```
INFO:my_project:Starting up
WARNING:my_project:Sensor returned unexpected value
ERROR:my_project:Could not connect to network
CRITICAL:my_project:Out of memory, halting
```

## Key things you can do

| What you want | How to do it |
|---|---|
| Create a logger | `logger = logging.getLogger("name")` |
| Set minimum level | `logger.setLevel(logging.DEBUG)` |
| Log at each level | `logger.debug()`, `.info()`, `.warning()`, `.error()`, `.critical()` |
| Write logs to a file | `logger.addHandler(logging.FileHandler("/log.txt"))` |
| Log to both REPL and file | Add both `StreamHandler` and `FileHandler` |
| Format the timestamp | `logging.FileHandler("/log.txt", mode="a")` — appends to existing file |
| Check current level | `logger.level` |

### Logging to a file

```python
import adafruit_logging as logging

logger = logging.getLogger("sensor_log")
logger.setLevel(logging.WARNING)

# Log to the CIRCUITPY filesystem
file_handler = logging.FileHandler("/errors.txt")
logger.addHandler(file_handler)

logger.warning("Temperature exceeded threshold: 42C")
```

The file persists across resets — useful for debugging problems that only happen when you're not watching the REPL.

## Reading the official docs

Full API reference including handlers and formatters:
[https://docs.circuitpython.org/projects/logging/en/latest/](https://docs.circuitpython.org/projects/logging/en/latest/)
