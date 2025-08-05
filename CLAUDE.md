# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a ZMK (Zephyr Mechanical Keyboard) firmware configuration repository for a Corne keyboard with Nice Nano controllers. ZMK is a modern keyboard firmware built on the Zephyr RTOS that provides wireless connectivity via Bluetooth Low Energy.

## Build System

### GitHub Actions Build
- Builds are automated via GitHub Actions using the ZMK build system
- The build matrix is defined in `build.yaml` which specifies board/shield combinations
- Current configuration builds for `nice_nano` board with `corne_left` and `corne_right` shields
- Builds are triggered on push, pull request, or manual workflow dispatch
- Firmware artifacts (.uf2 files) are generated and available for download from GitHub Actions

### Local Development
- This repository uses West (Zephyr's meta-tool) for dependency management
- West manifest is configured in `config/west.yml` pointing to the main ZMK repository
- No local build commands are typically needed as builds happen via GitHub Actions

## Configuration Architecture

### Key Files Structure
- `config/corne.keymap` - Main keymap definition with layers, combos, and key bindings
- `config/corne.conf` - Board-specific configuration settings (RGB, OLED, debounce, sleep)
- `config/west.yml` - West manifest for ZMK dependency management
- `build.yaml` - GitHub Actions build matrix configuration
- `zephyr/module.yml` - Zephyr module configuration for board definitions

### Keymap Structure
The keymap follows ZMK's device tree format:
- **Layer 0 (default_layer)**: Base QWERTY layout with mod-tap keys
- **Layer 1 (lower_layer)**: Numbers, navigation, media controls, and Bluetooth management
- **Layer 2 (raise_layer)**: Symbols, brackets, and special characters
- **Combos**: Key combinations for shortcuts like line selection and tab switching

### Configuration Features
- RGB underglow enabled (`CONFIG_ZMK_RGB_UNDERGLOW=y`)
- OLED display enabled (`CONFIG_ZMK_DISPLAY=y`)
- Custom debounce settings for key responsiveness
- Sleep disabled to prevent key sticking issues
- Bluetooth profiles configured for device switching

### Board Configuration
- Uses Nice Nano controllers (wireless-enabled Pro Micro compatible)
- Split keyboard configuration with separate left/right builds
- Empty `boards/shields/` directory suggests no custom board definitions

## Development Workflow

### Making Configuration Changes
1. Edit `config/corne.keymap` for keymap modifications
2. Edit `config/corne.conf` for feature toggles and settings
3. Commit changes to trigger automatic GitHub Actions build
4. Download firmware artifacts from the completed Actions run
5. Flash the appropriate .uf2 files to each keyboard half

### Bluetooth Management
- Layer 1 includes Bluetooth selection keys (`&bt BT_SEL 1-4`)
- Each keyboard half maintains separate Bluetooth profiles
- Use the keymap's BT selection combos to switch between paired devices

### Testing Changes
- No local testing framework - validation happens through actual flashing and usage
- GitHub Actions build success indicates syntactically correct configuration
- Runtime testing requires flashing firmware to physical keyboard