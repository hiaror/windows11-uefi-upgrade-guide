# Step 5: Troubleshooting

## MBR2GPT Validation Fails

**Error:** "MBR2GPT: Validation failed"

**Cause:** Too many primary partitions (more than 3) or an active recovery partition blocking conversion.

**Fix:**
1. Run `reagentc /info` to find which partition WinRE is on
2. Check partition contents before deleting anything
3. Delete only orphaned recovery partitions or OEM partitions
4. Re-run `mbr2gpt /validate /allowFullOS /disk:0`

---

## Windows Won't Boot After UEFI Switch

**Symptom:** Black screen or loops back to boot menu after switching to UEFI Only.

**Cause:** Boot order in BIOS still points to old MBR bootloader, or EFI boot entry is missing.

**Fix 1 — Check boot order:**
1. Press F1 to enter BIOS
2. Go to Startup → Boot
3. Move Windows Boot Manager to position 1
4. Press F10 to save

**Fix 2 — Repair boot files from USB:**
Boot from Windows 11 USB → Repair my PC → Troubleshoot → Advanced Options → Command Prompt

```cmd
diskpart
list volume
exit
```

Find the 100MB FAT32 Hidden volume (EFI partition), assign it letter S:

```cmd
diskpart
select volume X
assign letter=S
exit

bcdboot D:\Windows /s S: /f UEFI
```

Replace D: with your Windows partition letter.

---

## Setup Stuck at 46% Getting Updates

**Symptom:** Windows 11 setup hangs at 46% for more than 15 minutes.

**Fix:** Restart the Windows Update service without closing setup:

```powershell
net stop wuauserv
net start wuauserv
```

Progress should resume within a minute.

---

## "Keep Personal Files and Apps" is Greyed Out

**Cause:** ISO language doesn't match your installed Windows language.

**Fix:**
1. Check your Windows language:
```powershell
powershell -c "[CultureInfo]::InstalledUICulture"
```
2. Download the matching ISO from microsoft.com/software-download/windows11
3. Common mistake: "English International" = en-GB, not en-US

---

## Battery Not Detected After UEFI Switch

**Symptom:** Lenovo Vantage shows battery not detected after firmware changes.

**Fix:** Reset the battery driver:
1. Open Device Manager
2. Expand Batteries
3. Right-click Microsoft ACPI-Compliant Control Method Battery
4. Select Uninstall device
5. Reboot — Windows reinstalls the driver automatically

Note: This issue often resolves itself after the Windows 11 25H2 upgrade.

---

## Intel ME Firmware Fails via Auto-Update

**Symptom:** Lenovo Vantage or Windows Update repeatedly fails to install Intel Management Engine firmware.

**Fix:** Install manually:
1. Go to support.lenovo.com and enter your serial number
2. Navigate to Drivers & Software → Motherboard Devices
3. Download the Intel Management Engine Firmware .exe
4. Run as Administrator while plugged into AC power
5. System will show a black screen for up to 15 minutes during flash — do not interrupt

---

## Verify Final State

Run these after everything is complete:

```powershell
# Confirm UEFI
msinfo32

# Confirm Windows version
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion" | Select-Object DisplayVersion, CurrentBuild

# Confirm Secure Boot status
Confirm-SecureBootUEFI

# Confirm GPT disk
Get-Disk | Select-Object Number, PartitionStyle, FriendlyName
```

Expected results:
- BIOS Mode: UEFI
- DisplayVersion: 25H2
- CurrentBuild: 26200
- PartitionStyle: GPT
