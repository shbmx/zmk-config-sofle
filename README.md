# ZMK Config for Sofle Keyboard

This is a ZMK firmware configuration for the Sofle split keyboard, based on the QMK keymap from qmk-userspace.

## Features

- Layers: QWERTY, LOWER, RAISE (Mac), RAISE_PC (Windows/Linux), ADJUST, plus a PC overlay
- Mac shortcuts by default; toggle PC mode from ADJUST for Ctrl-based shortcuts
- Conditional layer (LOWER + RAISE = ADJUST, in either mode)
- Encoders: volume and page scroll; on RAISE, word jump and line up/down
- Encoder presses: left = mute, right = play/pause
- Caps word on RAISE
- OLED display with battery percentage

## Layers

### QWERTY (Base Layer)
Standard QWERTY layout with ESC, TAB, and modifier keys in expected positions.

![QWERTY Layer](images/qwerty_layer.svg)

### LOWER (Layer 1)
- Function keys (F1-F12)
- Number row
- Symbols and special characters

![LOWER Layer](images/lower_layer.svg)

### RAISE (Mac)
- Navigation keys (arrows, page up/down, line start/end with Cmd+Left/Right)
- Cmd shortcuts: undo, cut, copy, paste, save, new tab
- Word navigation and delete-word with Option
- Screenshots: Cmd+Shift+4 (area) and Cmd+Shift+5 (tool)
- Caps word: capitalises the next word, then turns itself off
- Encoders: left = word jump, right = line up/down

![RAISE Layer](images/raise_layer.svg)

### RAISE_PC (Windows/Linux)
Same layout with Ctrl shortcuts, Home/End, Insert, Print Screen and Menu. Active in
PC mode only.

![RAISE_PC Layer](images/raise_pc_layer.svg)

### PC mode
Toggle with the PC/MAC key on ADJUST. It is an overlay layer that only swaps the RAISE
key for RAISE_PC; everything else is unchanged. ZMK does not persist layer state, so the
keyboard starts in Mac mode after every power-up or deep sleep.

### ADJUST
System controls:
- Bluetooth profile management
- Media controls (volume, play/pause, next/prev)
- Bootloader access
- PC/Mac mode toggle

![ADJUST Layer](images/adjust_layer.svg)

## Building

1. Fork this repository
2. Enable GitHub Actions in your fork
3. Push changes to trigger automatic builds
4. Download firmware artifacts from the Actions tab

## Flashing

1. Download the firmware files from GitHub Actions artifacts
2. Put the keyboard into bootloader mode (use ADJUST layer bootloader key or physical reset)
3. Flash the appropriate .uf2 file to each half:
   - `sofle_left-nice_nano__zmk-zmk.uf2` for the left half
   - `sofle_right-nice_nano__zmk-zmk.uf2` for the right half
   - `settings_reset-nice_nano__zmk-zmk.uf2` clears stored settings and pairings (flash it to both halves, then reflash the normal firmware)

## Configuration

- Display is enabled by default
- Deep sleep after 60 minutes idle
- Bluetooth transmit power increased for better range
- Eager debouncing for responsive typing
- USB boot protocol, so the keyboard works in BIOS/UEFI menus when plugged in over USB

## Updating ZMK

ZMK is pinned to a specific commit in two places, which must match:

- `config/west.yml` → `revision`
- `.github/workflows/build.yml` → `build-user-config.yml@<commit>`

Tracking `main` broke this build once: upstream moved to Zephyr 4.1 and renamed the
board from `nice_nano_v2` to `nice_nano//zmk`. To update, bump both to a newer commit,
push to a branch, and flash only after CI goes green.

## Hardware

Configured for nice!nano v2 controllers (board target `nice_nano//zmk`). If using different controllers, update the `build.yaml` file accordingly.