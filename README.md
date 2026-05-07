# Windows 11 UEFI Upgrade Guide
### From Legacy BIOS + MBR to UEFI + GPT + Windows 11 25H2: A Real-World ThinkPad Migration

## Who This Is For
You're an IT engineer or tech enthusiast with a laptop stuck on Legacy BIOS and MBR partitioning. Windows 11 24H2 or 25H2 won't install cleanly. You've tried bypasses. You want a proper, permanent fix: not a fragile workaround.

This guide documents a real migration performed on a **Lenovo ThinkPad X1 Yoga 4th Gen (i7-8665U)** running Windows 11 23H2. After following these steps, the machine is running **Windows 11 25H2 fully supported, no bypasses needed**.

## What This Guide Covers
- Safely deleting an orphaned recovery partition to free an MBR slot
- Converting MBR to GPT using MBR2GPT without data loss
- Switching ThinkPad firmware from Legacy BIOS to UEFI
- Repairing UEFI boot files when Windows won't boot after the switch
- Performing a clean in-place upgrade to Windows 11 25H2 keeping all apps

## Prerequisites
- Windows 11 21H2 or later already installed
- At least 25GB free on C: drive
- A bootable Windows 11 USB (created with Rufus) as recovery backup
- Laptop plugged into AC power throughout

## Steps Overview
1. [Check your partition layout and convert MBR to GPT](docs/01-mbr-to-gpt-conversion.md)
2. [Switch firmware to UEFI](docs/02-uefi-bios-settings.md)
3. [Repair boot files if needed](docs/03-boot-repair.md)
4. [Upgrade to Windows 11 25H2](docs/04-25h2-inplace-upgrade.md)
5. [Troubleshooting common issues](docs/05-troubleshooting.md)

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

## Author
Himanshu Arora: Senior Project Delivery Engineer
- GitHub: [github.com/hiaror](https://github.com/hiaror)
- LinkedIn: [linkedin.com/in/himanshusac](https://linkedin.com/in/himanshusac)
- Website: [aroramsp.com](https://aroramsp.com)
