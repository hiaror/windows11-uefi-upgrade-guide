# Step 2: Switch Firmware to UEFI

## Why This Order Matters
MBR2GPT must run BEFORE you switch to UEFI. If you switch firmware first without converting the disk, the machine won't boot: UEFI looks for an EFI System Partition that doesn't exist yet on an MBR disk.

This is the most common mistake people make. Do Step 1 first, always.

## ThinkPad BIOS Access
Restart your laptop and press **F1** when the Lenovo logo appears.

## Settings to Change

Navigate to **Startup** tab:

| Setting | Change To |
|---|---|
| UEFI/Legacy Boot | UEFI Only |
| UEFI/Legacy Boot Priority | UEFI First |

Press **F10** to save and exit.

## Set Boot Order
If Windows doesn't boot automatically after switching to UEFI:

1. Press **F1** to enter BIOS again
2. Go to **Startup → Boot**
3. Move **Windows Boot Manager** to position 1 using the **+** key
4. Press **F10** to save

## Verify UEFI is Active
Once Windows loads, open System Information (msinfo32) and confirm:

```
BIOS Mode: UEFI
```

Or run in PowerShell:

```powershell
Confirm-SecureBootUEFI
```

Returns **False** if Secure Boot is off but UEFI is active. Returns **True** if Secure Boot is enabled.

## Secure Boot
Not required for Windows 11 25H2 to install or run. Optional but recommended for better security.

To enable: BIOS → Security → Secure Boot → Enabled.

See [Step 3](03-boot-repair.md) if Windows doesn't boot after the firmware switch.
