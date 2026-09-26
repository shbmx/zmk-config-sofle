# ZMK Config for Sofle Keyboard

This is a ZMK firmware configuration for the Sofle split keyboard, based on the QMK keymap from qmk-userspace.

## Features

- 4 layers: QWERTY, LOWER, RAISE, and ADJUST
- Conditional layer (LOWER + RAISE = ADJUST)
- Encoder support for volume and page scrolling
- OLED display with battery percentage
- ZMK Studio support for real-time keymap editing
- Optimized for macOS usage

## Layers

### QWERTY (Base Layer)
Standard QWERTY layout with ESC, TAB, and modifier keys in expected positions.

![QWERTY Layer](images/qwerty_layer.svg)

### LOWER (Layer 1)
- Function keys (F1-F12)
- Number row
- Symbols and special characters

![LOWER Layer](images/lower_layer.svg)

### RAISE (Layer 2)
- Navigation keys (arrows, page up/down, home/end)
- Text editing shortcuts (copy, paste, cut, undo)
- Word navigation (Ctrl+Left/Right)

![RAISE Layer](images/raise_layer.svg)

### ADJUST (Layer 3)
System controls:
- Bluetooth profile management
- Media controls (volume, play/pause, next/prev)
- Bootloader access
- ZMK Studio unlock

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

## ZMK Studio

Studio is enabled on the left (central) half only, via `cmake-args` in `build.yaml`.
The left half is built with the `studio-rpc-usb-uart` snippet, so Studio connects over
USB (web app or native app) as well as Bluetooth. Per the ZMK docs, Bluetooth Studio
works in the native apps and the Linux web app only; on macOS, plug in over USB or use
the native app. Unlock with the `&studio_unlock` key on the ADJUST layer.

## Updating ZMK

ZMK is pinned to a specific commit in two places, which must match:

- `config/west.yml` → `revision`
- `.github/workflows/build.yml` → `build-user-config.yml@<commit>`

Tracking `main` broke this build once: upstream moved to Zephyr 4.1 and renamed the
board from `nice_nano_v2` to `nice_nano//zmk`. To update, bump both to a newer commit,
push to a branch, and flash only after CI goes green.

## Hardware

Configured for nice!nano v2 controllers (board target `nice_nano//zmk`). If using different controllers, update the `build.yaml` file accordingly.