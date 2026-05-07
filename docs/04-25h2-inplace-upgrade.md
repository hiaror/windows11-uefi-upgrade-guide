# Step 4: Upgrade to Windows 11 25H2

## Prerequisites
Before starting, confirm:

```powershell
# Check UEFI is active
msinfo32
# BIOS Mode should show: UEFI

# Check free space (need at least 25GB)
Get-PSDrive C | Select-Object Used, Free

# Check Windows language
powershell -c "[CultureInfo]::InstalledUICulture"
```

Note your language output: the ISO you download must match exactly.

## Download the Correct ISO
Go to **microsoft.com/software-download/windows11**

Select:
- Windows 11 (multi-edition ISO for x64 devices)
- Language matching your installed Windows exactly

**Common mistake:** "English International" is en-GB (British English). If your Windows is en-US, download "English" not "English International". The installer will grey out "Keep personal files and apps" if there's a language mismatch.

## Verify the ISO
Always verify the hash before installing:

```powershell
Get-FileHash "D:\path\to\Win11_25H2_English_x64_v2.iso"
```

Compare the output against the SHA256 hash on Microsoft's download page.

## Mount and Run Setup
1. Right-click the ISO file
2. Select **Mount**
3. Open the mounted drive in File Explorer
4. Double-click **setup.exe**

## Setup Options
When asked about updates select **"Not right now"**

If it gets stuck at 46% "Getting updates":

```powershell
net stop wuauserv
net start wuauserv
```

This restarts the Windows Update service and usually unsticks the progress within a minute.

## Confirm Before Installing
On the "Ready to install" screen verify:

- Install Windows 11 Pro ✓
- Keep personal files and apps ✓

If "Keep personal files and apps" is greyed out, your ISO language doesn't match your Windows language. Stop, download the correct ISO, retry.

## Installation
Click **Install**. The process takes 1-2 hours with multiple reboots. Stay plugged into AC power throughout.

## Verify After Upgrade

```powershell
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion" | Select-Object ProductName, DisplayVersion, CurrentBuild, UBR
```

Expected output:
```
ProductName    : Windows 10 Pro  (this is normal, ignore it)
DisplayVersion : 25H2
CurrentBuild   : 26200
```

## Why "Windows 10 Pro" Shows in ProductName
Microsoft never updates the ProductName registry string during feature upgrades. It's a known quirk. The real version identifiers are DisplayVersion and CurrentBuild. Build 26200 = Windows 11 25H2.
