# ZMK XIAO BLE 2-Mode Hybrid Keyboard & Dongle Setup Guide

This branch (`feature/dynamic-role`) is pre-configured to build two firmware targets for the **Seeed Studio XIAO BLE** board:
1. **`xiao_test_dongle` (Dongle / Master):** Plugged into the PC. Acts as the split Central coordinator.
2. **`xiao_test` (Keyboard / Slave & Standalone Hybrid):** Acts as a split peripheral to the dongle in Dongle Mode, and switches to a standalone BLE/USB keyboard in Direct Mode.

Both configurations are built against your custom ZMK fork ([tienanhnguyen996/zmk](https://github.com/tienanhnguyen996/zmk)) on the `main` branch.

---

## Hardware Configuration (1 GPIO Pin = 1 Key)

### Keyboard Side (`xiao_test`):
* The key matrix is configured using a **Direct Keyscan** with a single key connected on pin **D0 (GPIO P0.02)** shorted to **GND**.
* Shorting D0 to GND acts as the key press.
* This key is intercepted locally. Pressing it toggles between **Dongle Mode** and **Direct Mode** on the fly.
* Active configurations are stored in `config/xiao_test.conf`:
  ```ini
  CONFIG_ZMK_UNIBODY_HYBRID=y
  CONFIG_ZMK_UNIBODY_TOGGLE_KEY_POSITION=0
  ```

### Dongle Side (`xiao_test_dongle`):
* Configured as a split Central with no physical keys (using a mock keyscan device).
* Activates split communication with `1` peripheral device.
* Active configurations are stored in `config/xiao_test_dongle.conf`:
  ```ini
  CONFIG_ZMK_SPLIT=y
  CONFIG_ZMK_SPLIT_ROLE_CENTRAL=y
  CONFIG_ZMK_SPLIT_BLE_CENTRAL_PERIPHERALS=1
  ```

---

## Build and Push to GitHub

Once you push this branch to your repository, GitHub Actions will compile both targets:
```bash
git add .
git commit -m "Configure build.yaml for seeeduino_xiao_ble targets"
git push origin feature/dynamic-role
```

You can download the built UF2 files from the **Actions** tab on your GitHub repository page:
* Flash `xiao_test_dongle-xiao_ble-zmk.uf2` to the Dongle board.
* Flash `xiao_test-xiao_ble-zmk.uf2` to the Keyboard board.
