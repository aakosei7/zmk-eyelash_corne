# Minimal display test

This test intentionally uses ZMK's built-in `nice_view` shield instead of the custom
`nice_view_gem` shield. The goal is to isolate only the LS0XX/SPI display path while
leaving the user's keymap, ZMK Studio configuration, split behavior, and keyboard
matrix definitions unchanged.

## What changed

- `build.yaml`: builds only the LEFT side with `eyelash_corne_left nice_view`.
- `config/west.yml`: removed the `zmk-nice-oled` module.
- `boards/shields/eyelash_corne/Kconfig.defconfig`: removed OLED-only forced `I2C`
  and `SSD1306` defaults.
- `eyelash_corne_left.conf`: removed the custom OLED status-screen selection and
  forced ZMK's built-in status screen.
- `eyelash_corne_right.conf`: removed stale `CONFIG_NICE_OLED_*` options for future
  cleanliness; the right side is NOT part of this test build.
- `eyelash_corne.dtsi`: unchanged. Existing display SPI wiring remains:
  - SCK: P0.20
  - MOSI: P0.17
  - MISO: P0.25
  - CS: P0.06
- `config/eyelash_corne.keymap`: unchanged.

## First hardware test

1. Push this repository/branch and wait for GitHub Actions to succeed.
2. Download the firmware artifact.
3. Flash ONLY `eyelash_corne_minimal_display_left.uf2` (or the UF2 generated under
   that artifact name) to the left half.
4. Do NOT run `settings_reset`.
5. Do NOT flash the right half.
6. Check:
   - whether anything appears on the left display;
   - normal key input;
   - all existing layers;
   - split communication with the unchanged right half;
   - ZMK Studio connectivity.

If the display works, the next step is to reintroduce `nice_view_gem` custom UI only,
without changing SPI wiring or the keymap.
