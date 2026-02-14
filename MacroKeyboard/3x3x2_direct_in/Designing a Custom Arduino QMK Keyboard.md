# Designing a Custom Arduino QMK Keyboard

This README details the complete process for building a custom 3x3x2 direct-input (3x6 matrix) macro keyboard using Arduino-compatible hardware like Pro Micro and QMK Firmware. It covers mechanics, PCB visuals, JSON configuration, environment setup, keymap creation, and visual design tools. Follow these steps to go from prototype to flashing custom firmware.[^1]

## Visual References

Key images showcase the physical build:

- **Keyboard Internals**: `MacroKeyboard\3x3x2_direct_in\Keyboard Inside.jpg` – Reveals switch mounting, wiring to Pro Micro pins (ATmega32u4), and direct matrix connections without diodes.
- **Top Layout**: `MacroKeyboard\3x3x2_direct_in\Keyboard Layout.jpg` – Shows the 3x6 ortholinear grid for macros/numpad use.
- **Pro Micro Pinout**: `MacroKeyboard\pro_micro_pinout.jpg` – Essential reference matching JSON pins (e.g., D0-D7, B1-B6, F4-F7).

These confirm a compact, handwired design using standard mechanical switches.

## QMK JSON Configuration

The core hardware spec lives in `..\qmk_firmware\keyboards\3x3x2_direct_in\keyboard.json`:

```
{
  "manufacturer": "custom",
  "keyboard_name": "3x3x2_direct_in",
  // ... full details as previously explained: 3x6 direct matrix, ATmega32u4, etc.
}
```

This file defines pinouts, USB VID/PID, and layout grid for QMK to generate matrix scanning code.[^2]

## QMK Repo and MSYS Setup

Start with QMK Firmware repo for all supported keyboards and tools.

1. Download/install **QMK MSYS** (Windows bundle with MSYS2, CLI, dependencies) from official releases.[^1]
2. Launch **QMK MSYS** terminal (blue MinGW64 icon).
3. Run `qmk setup` – Answers "y" to prompts; clones repo to `~/qmk_firmware`, sets up Git/Discord integration.
4. Test: `qmk list-keyboards` lists options; compile default like `qmk compile -kb clueboard/66/rev3 -km default`.[^3]

This environment persists for all builds, handling ARM/AVR toolchains.

## From QMK MSYS to JSON and keymap.c

With MSYS ready, add your custom keyboard:

1. **Add Keyboard Folder**: In `qmk_firmware/keyboards/`, create `3x3x2_direct_in/` and paste `keyboard.json`.
2. **Create Keymap**: Run `qmk new-keymap -kb 3x3x2_direct_in -km default` (or your name). Generates `keymaps/default/keymap.c`.
3. **Design Keymap** (detailed below).
4. **Compile**: `qmk compile -kb 3x3x2_direct_in -km default` → Outputs `.hex` (e.g., 26KB/28KB size check passes).[^4]
5. **Flash**: Use `qmk flash` or Caterina bootloader via `avrdude` on reset.[^1]

JSON defines hardware/matrix; `keymap.c` overrides with layers (e.g., `const uint16_t PROGMEM keymaps[][MATRIX_ROWS][MATRIX_COLS] = { [^0] = LAYOUT(...), };`). Edit `keymap.c` for macros, RGB, etc.[^5]

## Using Keyboard Layout Editor for Button Text

Visualize and generate keycaps/printouts:

1. Visit [Keyboard Layout Editor](https://www.keyboard-layout-editor.com/#/) – Drag to 3x6 grid matching JSON layout (x:0-5, y:0-2).
2. **Sample Text Layout** (export as SVG/PNG for stencils):
```
Layer 0 (Macros):
┌───┬───┬───┬───┬───┬───┐
│F1 │F2 │F3 │F4 │F5 │F6 │
├───┼───┼───┼───┼───┼───┤
│M1 │M2 │M3 │M4 │M5 │M6 │
├───┼───┼───┼───┼───┼───┤
│M7 │M8 │M9 │M10│M11│M12│
└───┴───┴───┴───┴───┴───┘

Layer 1 (Nums):
┌───┬───┬───┬───┬───┬───┐
│ 1 │ 2 │ 3 │ + │Esc│Del│
├───┼───┼───┼───┼───┼───┤
│ 4 │ 5 │ 6 │ - │←  │→  │
├───┼───┼───┼───┼───┼───┤
│ 7 │ 8 │ 9 │ * │/  │=  │
└───┴───┴───┴───┴───┴───┘
```

3. Export JSON → Import to QMK Configurator (config.qmk.fm) for `.c` conversion via tools like json2c or online converters.[^6]
4. Convert SVG via [PicSVG](https://picsvg.com/) for clean vectors; print/etch key legends.[^5]

## Build and Troubleshooting

- **Firmware Guide**: [QMK Build FAQ](https://docs.qmk.fm/faq_build) – Covers errors, sizes, flashing.
- Common Issues: Pin conflicts (check pinout.jpg), matrix debounce; test with `qmk inspect`.
- Repo: [QMK GitHub](https://github.com/qmk/qmk_firmware) – Fork for customs.

This workflow yields a fully custom macro pad ready for Arduino flashing. Expand layers in `keymap.c` for more functionality!
<span style="display:none">[^10][^11][^12][^13][^14][^15][^7][^8][^9]</span>

<div align="center">⁂</div>

[^1]: https://docs.qmk.fm/newbs_getting_started

[^2]: https://docs.qmk.fm/config_options

[^3]: https://docs.qmk.fm/configurator_step_by_step

[^4]: https://docs.qmk.fm/newbs_building_firmware

[^5]: https://github.com/jhelvy/qmkJsonConverter

[^6]: https://www.reddit.com/r/MechanicalKeyboards/comments/eslu3n/is_there_a_keyboard_layout_editor_that_generates/

[^7]: https://www.reddit.com/r/MechanicalKeyboards/comments/g4spdj/app_for_converting_qmk_configurator_json_to/

[^8]: https://www.reddit.com/r/olkb/comments/p8juz7/how_to_use_own_keymapjson_on_the_qmk_configurator/

[^9]: https://docs.qmk.fm/configurator_default_keymaps

[^10]: https://www.reddit.com/r/olkb/comments/1ear9aw/best_way_to_create_a_configh_file_when_i_only/

[^11]: https://www.reddit.com/r/olkb/comments/k38uu7/from_keymapc_to_keyboard_layout_editor/

[^12]: https://groups.google.com/g/faslareper/c/mmViqWCm28U

[^13]: https://blueprint.hackclub.com/resources/qmk

[^14]: https://vgpena.github.io/qmk/

[^15]: https://msys.qmk.fm/guide

