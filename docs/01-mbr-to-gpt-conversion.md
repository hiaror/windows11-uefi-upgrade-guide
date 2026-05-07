# Step 1: MBR to GPT Conversion

## Background
MBR (Master Boot Record) is a legacy disk format from the 1980s. It has a hard limit of 4 primary partitions and only works with Legacy BIOS firmware. GPT (GUID Partition Table) is the modern replacement: it works with UEFI and supports up to 128 partitions.

Windows 11 24H2 and later dropped Legacy BIOS/MBR boot support entirely. Converting to GPT is the only clean path forward.

## Check Your Current Partition Layout

Open PowerShell as Administrator and run:

```powershell
diskpart
list disk
select disk 0
list partition
exit
```

Note down all partitions: their type, size, and offset.

## Identify What WinRE Is Using

```powershell
reagentc /info
```

This tells you which partition Windows Recovery Environment lives on. Do NOT delete that partition.

## The MBR2GPT Problem
MBR2GPT requires 3 or fewer primary partitions. Most machines have 4. You need to delete one before conversion.

**Safe deletion candidates:**
- Orphaned/old recovery partitions not referenced by reagentc /info
- OEM vendor partitions (large, ~10-20GB, not system or Windows)

**Never delete:**
- System Reserved (active boot partition)
- C: drive (Windows)
- The active WinRE partition shown by reagentc /info

## Check Contents Before Deleting

Assign a temporary drive letter to inspect:

```powershell
diskpart
select disk 0
select partition X  # replace X with your partition number
assign letter=Z
exit

cmd /c "dir Z:\ /a"
cmd /c "dir Z:\Recovery /a /s"
```

If it contains an old Winre.wim dated years ago and reagentc /info points elsewhere, it's safe to delete.

## Delete the Orphaned Partition

```powershell
diskpart
select disk 0
select partition X
remove letter=Z
delete partition override
exit
```

Verify you're down to 3 partitions:

```powershell
diskpart
select disk 0
list partition
exit
```

## Run MBR2GPT

Validate first, never skip this:

```powershell
mbr2gpt /validate /allowFullOS /disk:0
```

Only proceed if you see **"Validation completed successfully"**. Then:

```powershell
mbr2gpt /convert /allowFullOS /disk:0
```

This creates the EFI System Partition automatically and installs UEFI boot files. Your data is untouched.

## What MBR2GPT Actually Does
1. Reads your existing MBR partition table
2. Creates a new GPT partition table
3. Carves out a small EFI System Partition (~100MB FAT32)
4. Installs Windows UEFI boot files into the ESP
5. Rewrites the partition layout without touching your data

Do NOT reboot yet. Switch firmware to UEFI first, see [Step 2](02-uefi-bios-settings.md).