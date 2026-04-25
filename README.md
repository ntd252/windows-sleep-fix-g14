# Windows Sleep Fix - Asus Zephyrus G14

Comprehensive investigations, discussions, and solutions for Windows Modern Standby stability issues on the **Asus Zephyrus G14** laptop.

## Overview

The Asus Zephyrus G14 (particularly 2024-2025 models like the GA401QEC and GA403WR) has experienced widespread sleep/wake issues when using Windows Modern Standby. The most common symptom is the system hanging on wake, requiring forced reboots.

This repository consolidates:

- **Discussions** from the Zephyrus G14 community
- **Root cause analyses** of known issues
- **Verified solutions** and workarounds
- **Registry settings** and configuration tweaks
- **Event logs** for troubleshooting

---

## Table of Contents

1. [Known Issues](#known-issues)
2. [Discussions & Case Studies](#discussions--case-studies)
3. [Solutions](#solutions)
4. [Registry Tweaks](#registry-tweaks)
5. [Troubleshooting](#troubleshooting)
6. [Contributing](#contributing)

---

## Known Issues

### Kernel-PnP Error (EventID: 219)

- **Symptom:** Laptop hangs on wake from sleep or doesn't wake at all
- **Impact:** Requires forced reboot (hold power button)
- **Root Cause:** Windows User-Mode Driver Framework (WUDFRd) crashes while re-initializing keyboard power state on ACPI wake
- **Affected Models:** GA401QEC, GA403WR, 2025 G14 with 5070Ti
- **Core Issue:** Conflict between keyboard power management and Windows Modern Standby

---

## Discussions & Case Studies

### [Kernel-PnP Sleep Hang Fix](./discussions/reddit-g14-kernel-pnp-sleep-hang-fix.md)

**Source:** [r/ZephyrusG14 Discussion](https://www.reddit.com/r/ZephyrusG14/comments/1sryuka/fix_zephyrus_g14_kernelpnp_errors_and_hangups/)

A detailed investigation into the Kernel-PnP error and the solution that fixed it by disabling keyboard power management. Includes:

- Problem description and symptoms
- Root cause analysis
- Step-by-step fix using Device Manager
- Results and verification

---

## Solutions

### Solution 1: Disable Keyboard Power Management

**Status:** ✅ Verified Fix

**How it works:**
Prevents Windows from disabling the keyboard during sleep, which was triggering the WUDFRd crash.

**Steps:**

1. Open Device Manager
2. Switch to **View → Devices by connection**
3. Navigate: `ACPI x64 based PC → ... → USB Input Device`
4. Properties → Power Management
5. Uncheck: "Allow the computer to turn off this device to save power"
6. Reboot

For detailed instructions, see: [Kernel-PnP Sleep Hang Fix](./discussions/reddit-g14-kernel-pnp-sleep-hang-fix.md)

---

## Registry Tweaks

### Modern Standby Optimization Registry Files

The repository includes several `.reg` files for configuring Windows Modern Standby:

- **Always_Disable_Modern_Standby_network_connectivity_On_Battery.reg** - Disables network during sleep when on battery
- **Always_Disable_Modern_Standby_network_connectivity_when_Plugged_In.reg** - Disables network during sleep when plugged in
- **Default_Not_Configured_Modern_Standby_network_connectivity_when_Plugged_In.reg** - Restores default settings

**Usage:** Double-click to apply or import via Registry Editor

⚠️ **Warning:** Backup your registry before applying changes.

---

## Troubleshooting

### Event Viewer Diagnostics

To check if you're experiencing the same issue:

1. Open **Event Viewer**
2. Navigate to: Windows Logs → System
3. Look for errors with:
    - Source: `Microsoft-Windows-PnP/Driver Framework`
    - EventID: `219`
    - Task: `212`

### Symptoms to Check

- [ ] Laptop doesn't wake from sleep
- [ ] Screen remains black after wake attempts
- [ ] Forced reboot required (hold power button)
- [ ] Occurs intermittently or consistently
- [ ] Happens in idle/automatic sleep mode

---

## Contributing

Have you experienced or fixed similar issues on your G14? Contributions are welcome!

- **Report new issues:** Create a discussion or document your case
- **Share workarounds:** Add verified solutions to the discussions folder
- **Improve docs:** Submit updates to improve clarity and completeness

---

## Resources

- **Asus Zephyrus G14 Reddit:** https://reddit.com/r/ZephyrusG14
- **Windows Modern Standby:** [Microsoft Docs](https://learn.microsoft.com/en-us/windows-hardware/design/device-experiences/modern-standby)
- **Windows Event Viewer:** Built-in Windows diagnostic tool

---

## Disclaimer

These solutions are provided as-is based on community investigations. Always backup important data and registry settings before implementing changes. Test thoroughly before relying on any workaround in production use.

**Last Updated:** April 2026
