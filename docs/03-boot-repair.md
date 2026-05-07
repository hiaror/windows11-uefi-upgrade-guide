# Step 3: Boot Repair

## When You Need This
After switching to UEFI, Windows shows a black screen or loops back to the boot menu without loading. This happens when the EFI boot entry exists but the boot files are missing or pointing to the wrong partition.

This is not data loss. Your Windows installation is intact.

## What You Need
A bootable Windows 11 USB created with Rufus. Boot from it and select **Repair my PC**, do NOT click Install.

## Access Command Prompt from USB
1. Boot from the Windows 11 USB
2. On the language screen click **Next**
3. On the next screen select **Repair my PC**
4. Click **Troubleshoot**
5. Click **Advanced options**
6. Click **Command Prompt**

## Find Your Partitions

```cmd
diskpart
list volume
exit
```

Look for:
- Your Windows partition (largest NTFS, usually C: or D:)
- The EFI System Partition (100MB FAT32, marked Hidden)

Note the drive letters assigned. In WinRE the letters differ from your normal Windows session.

## Assign a Letter to the EFI Partition

```cmd
diskpart
select disk 0
select volume X
assign letter=S
exit
```

Replace X with the volume number of the 100MB FAT32 Hidden partition.

## Repair Boot Files

```cmd
bcdboot D:\Windows /s S: /f UEFI
```

Replace D: with the letter of your Windows partition.

If successful you'll see: **Boot files successfully created.**

## Reboot

```cmd
exit
```

Click **Continue** to exit and boot into Windows. Remove the USB when it restarts.

## Verify After Boot
Once on the desktop run:

```powershell
msinfo32
```

Confirm **BIOS Mode: UEFI** and **Boot Device: \Device\HarddiskVolume3** (or similar EFI volume).

## Why This Happens
MBR2GPT creates the EFI partition and installs boot files correctly. But sometimes the firmware's boot entry doesn't update automatically, or the boot order in BIOS still points to the old MBR bootloader. The bcdboot command rebuilds the UEFI boot entry from scratch pointing to the correct EFI partition.