# Reference

This section is a library-by-library reference for CircuitPython. Each page covers one library: what it does, how to install it, basic usage, and links to the official Adafruit guide. The reference section is designed to be used alongside the Projects section — project pages link here when they use a library, so you can get more detail without hunting for it.

---

## How this section is organized

Each library page follows the same structure:

- **What it does** — a plain-English description of the library's purpose
- **Installing the library** — how to get it onto your board
- **Quick start** — a minimal working example
- **Key things you can do** — a table of common tasks and how to accomplish them
- **Reading the official docs** — what to look for in the documentation and what the important options are
- **Projects using this library** — links to Adafruit guides and other projects that use it

---

## How to install a library

CircuitPython libraries are distributed as `.mpy` files (precompiled Python) or as folders. They live in the `lib/` folder on your board. Here is the process:

1. Go to [circuitpython.org/libraries](https://circuitpython.org/libraries) and download the CircuitPython Bundle. Make sure the bundle version matches your CircuitPython version — check the version number in `boot_out.txt` on your board if you are not sure.
2. Unzip the bundle. Inside you will find a `lib/` folder containing every available library.
3. Find the `.mpy` file (or folder) for the library you need. Library names match what you import in your code — for example, `neopixel.mpy` is what you need to `import neopixel`.
4. Copy that file or folder to the `lib/` folder on your board.
5. Import it in your code as you normally would.

You do not need to copy the entire bundle — only the libraries your project actually uses. Boards have limited storage, so copying only what you need is good practice.

---

## How to read official documentation

This is a skill worth developing. Official documentation can be intimidating at first, but once you know what you are looking at it becomes a fast way to answer questions.

### Where the docs live

- [docs.circuitpython.org](https://docs.circuitpython.org) — the main CircuitPython documentation hub
- [circuitpython.readthedocs.io](https://circuitpython.readthedocs.io) — same content, alternative URL
- Individual library docs are linked from each library page in this reference section

### What the docs are (and are not)

Official documentation is a **reference**, not a tutorial. It lists every method, parameter, and property a library has, but it rarely explains *why* you would use something or shows you a full working project. That is what Adafruit guides are for (more on that below).

The best approach is:

1. Start with a project — either in the Projects section of this wiki or an Adafruit Learn guide
2. Get the project working
3. Come back to the official docs to explore what else the library can do

Trying to learn a library by reading the docs first, before you have used it at all, is usually frustrating. The docs make much more sense once you have seen the library in action.

### What a "class" means in this context

When you write something like:

```python
pixels = neopixel.NeoPixel(board.D4, 8, brightness=0.3)
```

you are creating an **object**. `NeoPixel` is a class — think of it as a template. When you call `neopixel.NeoPixel(...)` you get back an object (stored here as `pixels`) that represents your specific strip of LEDs.

The official docs are organized around classes. When you look up the `neopixel` library, you will see a section for the `NeoPixel` class. Under it, you will find:

- The **constructor** — how to create the object (what arguments `neopixel.NeoPixel(...)` accepts)
- **Methods** — functions you call on the object, like `pixels.fill(...)`
- **Properties** — values you can read or set, like `pixels.brightness`

### Reading a method signature

Docs list methods in a compact format. Here is an example of what you might see:

```
fill(color)
    Colors all pixels the given color.

    Parameters: color (tuple) – An RGB tuple (r, g, b) or RGBW tuple (r, g, b, w)
```

Plain-English translation: call `pixels.fill(...)` and pass in a color as a tuple of three numbers — red, green, blue — each between 0 and 255. For example, `pixels.fill((255, 0, 0))` fills everything red.

Some method signatures look more cryptic:

```
__setitem__(index, val)
```

This is Python's way of writing "the thing that happens when you use square brackets." When you write `pixels[0] = (0, 255, 0)`, you are calling `__setitem__`. You do not call it directly — you just use the bracket syntax. Seeing `__setitem__` in the docs means the object supports `object[index] = value`.

---

## Adafruit guides vs official docs

These two resources do different things and both are worth knowing about.

**Adafruit Learn guides** at [learn.adafruit.com](https://learn.adafruit.com) are **tutorials**. They walk you through a project from wiring to finished code. They explain context, show photos, and are written for people who are learning. Start here when you are new to a library or want to build something specific.

**Official docs** are **references**. Once you know the basics of a library and want to know "does this library have a way to do X?", the docs are where you look. They are dense and assume you already know what the library is for — but they are complete and accurate.

Think of it this way: Adafruit guides teach you to drive. The official docs are the owner's manual.
