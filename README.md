# Windows 11 UEFI Upgrade Guide

## Overview
A real-world migration from Legacy BIOS + MBR to UEFI + GPT + Windows 11 25H2, performed on a **Lenovo ThinkPad X1 Yoga 4th Gen (i7-8665U)** running Windows 11 23H2. After following these steps, the machine is running **Windows 11 25H2 fully supported, no bypasses needed**.

## Who This Is For
You're an IT engineer or tech enthusiast with a laptop stuck on Legacy BIOS and MBR partitioning. Windows 11 24H2 or 25H2 won't install cleanly. You've tried bypasses. You want a proper, permanent fix, not a fragile workaround.

## What This Guide Covers
- Safely deleting an orphaned recovery partition to free an MBR slot
- Converting MBR to GPT using MBR2GPT without data loss
- Switching ThinkPad firmware from Legacy BIOS to UEFI
- Repairing UEFI boot files when Windows won't boot after the switch
- Performing a clean in-place upgrade to Windows 11 25H2 keeping all apps

## Repository Structure
```
.
├── docs/
│   ├── 01-mbr-to-gpt-conversion.md
│   ├── 02-uefi-bios-settings.md
│   ├── 03-boot-repair.md
│   ├── 04-25h2-inplace-upgrade.md
│   └── 05-troubleshooting.md
└── README.md
```

## Prerequisites
- Windows 11 21H2 or later already installed
- At least 25GB free on C: drive
- A bootable Windows 11 USB (created with Rufus) as recovery backup
- Laptop plugged into AC power throughout

## Steps Overview
1. [Check your partition layout and identify safe deletion candidates](docs/01-mbr-to-gpt-conversion.md)
2. [Convert MBR to GPT](docs/01-mbr-to-gpt-conversion.md)
3. [Switch firmware to UEFI](docs/02-uefi-bios-settings.md)
4. [Repair boot files if needed](docs/03-boot-repair.md)
5. [Upgrade to Windows 11 25H2](docs/04-25h2-inplace-upgrade.md)
6. [Troubleshooting common issues](docs/05-troubleshooting.md)

## Hardware Used
| Component | Detail |
|---|---|
| Device | Lenovo ThinkPad X1 Yoga 4th Gen |
| CPU | Intel i7-8665U |
| RAM | 16GB |
| Storage | Samsung NVMe 1TB |
| Original OS | Windows 11 23H2 (Build 22631) |
| Final OS | Windows 11 25H2 (Build 26200) |

## Result
No data loss. No app reinstallation. Fully supported upgrade path going forward.

## Safety Notes
This procedure modifies partition tables and firmware boot mode. Skipping a step or running them out of order can leave the machine unbootable. Always:
- Take a full image backup before starting
- Have the bootable Windows 11 USB ready before you begin
- Run MBR2GPT before switching firmware to UEFI, never after
- Do not interrupt the firmware switch or the in-place upgrade

## Disclaimer
This guide documents a specific real-world migration on Lenovo ThinkPad hardware. Steps and BIOS menu paths may differ on other vendors and models. Provided as-is for reference and learning purposes. Review and adapt before applying to production hardware.

## Blog Post

A full write-up of the script, the BIOS conversion process, and the edge cases is at [AroraMSP: Upgrading Windows 11 on non-UEFI devices with PowerShell](https://aroramsp.com/blog/windows11-uefi-upgrade).
