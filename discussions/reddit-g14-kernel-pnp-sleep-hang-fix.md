# Fix Zephyrus G14 Kernel-PnP Errors and Sleep Hang-ups

**Model:** 2025 G14 5070ti

## Problem Description

For the past few months, my Zephyrus G14 has been experiencing critical sleep/wake issues:

- **Symptom:** Laptop doesn't turn back ON after going to sleep in idle mode
- **Workaround:** Hard reboot required by holding the power button
- **Frequency:** Intermittent but recurring issue

After investigating, I discovered many users facing the same problem. The Event Viewer revealed a consistent error across the G14 community:

> **Kernel-PnP Error**  
> EventID: 219 | TaskCategory: 212

_See: [r/ZephyrusG14 - Original Discussion](https://www.reddit.com/r/ZephyrusG14/comments/1sryuka/fix_zephyrus_g14_kernelpnp_errors_and_hangups/)_

## Root Cause Analysis

Upon further investigation, I identified the underlying issue:

When the Zephyrus G14 attempts to wake from sleep, the **Windows User-Mode Driver Framework** (`\Driver\WUDFRd`) crashes or times out while reinitializing the keyboard's power state. This hangs the entire ACPI wake process and forces a hard reboot.

**Affected Models:** GA401QEC, GA403WR, and others

**Known Conflict:** Keyboard power management vs. Windows Modern Standby

_See: [r/ZephyrusG14 - Original Discussion](https://www.reddit.com/r/ZephyrusG14/comments/1sryuka/fix_zephyrus_g14_kernelpnp_errors_and_hangups/)_

## Solution: Disable Keyboard Power Management

### Why This Works

By disabling power management for the keyboard input device, we prevent Windows from turning it off to save power—which was triggering the WUDFD crash.

### Challenge

The HID Keyboard Input Device doesn't have a Power Management tab directly accessible in Device Manager.

**Workaround:** Access power management through the device's parent device.

### Step-by-Step Fix

1. Open **Device Manager**
2. Switch to **View** → **Devices by connection**
3. Navigate through the device tree following this path:

    ```
    ACPI x64 based PC
    ├── Microsoft ACPI Complaint System
    ├── PCIE Express Root Complex
    ├── PCIE Express Root Port (check all ports)
    ├── AMD 3.1 Extensible Host Controller 1.20
    ├── USB Root Hub 3.0
    ├── USB Input Device  ← SELECT THIS
    └── HID Keyboard Device
    ```

4. Right-click on **USB Input Device** and select **Properties**
5. Go to the **Power Management** tab
6. **Uncheck** the option: _"Allow the computer to turn off this device to save power"_
7. Click **OK**
8. **Reboot** your laptop

_See: [r/ZephyrusG14 - Original Discussion](https://www.reddit.com/r/ZephyrusG14/comments/1sryuka/fix_zephyrus_g14_kernelpnp_errors_and_hangups/)_

## Results

After applying this fix, the Kernel-PnP errors and sleep hang-ups no longer occur.

---

**Hope this helps!** If you're experiencing similar issues on your Zephyrus G14, try this solution. Please report your results in the community.
