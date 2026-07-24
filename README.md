# ZMK Reviung41 Dynamic 2-Mode Configuration Setup Guide

This branch (`feature/dynamic-role`) is configured to use the custom **Dynamic 2-Mode Hybrid Keyboard ZMK fork** (located at [tienanhnguyen996/zmk](https://github.com/tienanhnguyen996/zmk)) instead of the upstream ZMK firmware.

This configuration enables your Reviung41 unibody keyboard to switch between:
1. **Dongle Mode (Split Peripheral):** Keypresses are wirelessly sent to a USB Dongle (Master), which is plugged into your computer.
2. **Direct Mode (Standalone BLE/USB):** Keyboard connects directly to your PC, tablet, or phone via Bluetooth or USB.

---

## How to Set Up Your Configuration

### 1. Enable Hybrid Mode in `reviung41.conf`
Open `config/reviung41.conf` and add the following lines to enable the hybrid configuration and select a physical key position to act as the mode-toggle key:

```ini
# Enable Hybrid Dynamic Role configuration
CONFIG_ZMK_UNIBODY_HYBRID=y

# Select a key position index on your Reviung41 keymap to toggle the mode
# Key position 38 is typically Lower-layer / center spacer area on Reviung41
CONFIG_ZMK_UNIBODY_TOGGLE_KEY_POSITION=38
```

> [!IMPORTANT]
> The key position you assign to `CONFIG_ZMK_UNIBODY_TOGGLE_KEY_POSITION` will be **intercepted locally** at the hardware level. When pressed, it will toggle the mode between Dongle and Direct. It will **not** trigger standard keys or custom keymap behaviors in either mode. Choose a key position that you do not mind dedicating to mode switching.

### 2. Verify `west.yml` (Pre-configured on this branch)
Your `config/west.yml` has already been updated to pull ZMK source code from your custom fork containing the implementation:
```yaml
manifest:
  remotes:
    - name: tienanhnguyen996
      url-base: https://github.com/tienanhnguyen996
  projects:
    - name: zmk
      remote: tienanhnguyen996
      revision: main
      import: app/west.yml
  self:
    path: config
```

### 3. Build & Flash
When you push this branch to GitHub, your **GitHub Actions Workflow** will automatically build the firmware using your custom ZMK fork. 

1. Push this branch to GitHub:
   ```bash
   git push origin feature/dynamic-role
   ```
2. Go to the **Actions** tab on your GitHub repository.
3. Download the built zip artifact containing the `.uf2` file.
4. Put your Reviung41 keyboard into bootloader mode and flash the `.uf2` file.
