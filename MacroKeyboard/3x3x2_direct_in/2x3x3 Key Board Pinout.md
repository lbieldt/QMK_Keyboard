# JSON Explain and Pinout

*"manufacturer": "custom",
  "keyboard_name": "3x3x2_direct_in",
  "maintainer": "custom",
  "processor": "atmega32u4",
  "bootloader": "caterina",
  "usb": {
    "vid": "0xFEED",
    "pid": "0x0001",
    "device_version": "1.0.0"
  },
  "matrix": {
    "rows": 3,
    "cols": 6
  },
  "matrix_pins": {
    "direct": [
      ["D0","D1","D3","D2","F5","F4"],
      ["D4","C6","B4","B3","F7","F6"],
      ["D7","E6","B5","B6","B2","B1"]
    ]
  },
  "layouts": {
    "LAYOUT": {
      "layout": [
        {"matrix":[0,0], "x":0, "y":0}, {"matrix":[0,1], "x":1, "y":0}, {"matrix":[0,2], "x":2, "y":0},
        {"matrix":[0,3], "x":3, "y":0}, {"matrix":[0,4], "x":4, "y":0}, {"matrix":[0,5], "x":5, "y":0},
        {"matrix":[1,0], "x":0, "y":1}, {"matrix":[1,1], "x":1, "y":1}, {"matrix":[1,2], "x":2, "y":1},
        {"matrix":[1,3], "x":3, "y":1}, {"matrix":[1,4], "x":4, "y":1}, {"matrix":[1,5], "x":5, "y":1},
        {"matrix":[2,0], "x":0, "y":2}, {"matrix":[2,1], "x":1, "y":2}, {"matrix":[2,2], "x":2, "y":2},
        {"matrix":[2,3], "x":3, "y":2}, {"matrix":[2,4], "x":4, "y":2}, {"matrix":[2,5], "x":5, "y":2}
      ]
    }
  }
}*

This JSON configuration defines a custom 3x6 matrix keyboard (3 rows, 6 columns) for QMK Firmware, a popular open-source keyboard firmware. It specifies hardware details like the ATmega32u4 microcontroller and direct pin mappings for a compact "3x3x2_direct_in" layout, likely a 3-layer split or numpad-style board.

## Core Metadata

Basic identifiers and hardware specs set the foundation for firmware compilation.

- Manufacturer and maintainer are "custom," indicating a personal/DIY project.
- Processor is ATmega32u4 (common for USB keyboards like Pro Micro clones), with Caterina bootloader for easy flashing.
- USB VID/PID (0xFEED/0x0001) uses QMK's default vendor/product IDs, version 1.0.0.[ from prior context on Markdown/code]


## Matrix Setup

Defines a direct-drive 3x6 grid (18 switches total) without scanning diodes.

```
"rows": 3,
"cols": 6
```

Pins are wired row-by-row to specific ATmega32u4 GPIO pins (D0-D7, C6, E6, B1-B6, F4-F7).


| Row | Column Pins (Direct Drive) |
| :-- | :-- |
| 0 | D0, D1, D3, D2, F5, F4 |
| 1 | D4, C6, B4, B3, F7, F6 |
| 2 | D7, E6, B5, B6, B2, B1 |

This maps physical switches to matrix coordinates (e.g.,  is row 2, col 5 on pin B1).

## Key Layout

The single "LAYOUT" defines 18 positions in a standard 3x6 grid (x=0-5 horizontal, y=0-2 vertical).

- Each key references its matrix position (e.g., {"matrix":} is top-left).
- No staggering or offsets; it's a flat grid, ideal for ortholinear or macropad builds.
- In QMK, this generates the `layout()` function for keymap arrays.

This config enables compiling QMK firmware via `qmk compile -kb <name> -km default`, producing a .hex file for the bootloader. For a visual, imagine a grid like this (matrix coords labeled):

```
[0,0][0,1][0,2][0,3][0,4][0,5]  ← Row y=0
[1,0][1,1][1,2][1,3][1,4][1,5]  ← Row y=1
[2,0][2,1][2,2][2,3][2,4][2,5]  ← Row y=2
```

<div align="center">⁂</div>
