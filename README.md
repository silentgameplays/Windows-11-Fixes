# Windows-11-Fixes
Windows 11 collection of Fixes

# 1. To enable a local account:

* Shift+F10

* ``OOBE\BYPASSNRO``

# New method added

* Shift+F10

* ``WinJS.Application.restart("ms-cxh://LOCALONLY")``

# or

* ``"start ms-cxh:localonly"``

# 2. Disable Bitlocker:

Shift + F10

* ``regedit``

* ``HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\BitLocker``

Click New and select DWORD (32-bit) value with a name PreventDeviceEncryption set it to 1.

# 3. Disable Recall AI:

Run cmd or powershell

* ``DISM /Online /Get-Featureinfo /Featurename:Recall``

* ``DISM /Online /Disable-Feature /Featurename:Recall``

# 4. Install Windows 11 On a Virtual Machine or Unsupported Hardware:

* Shift+F10

``regedit``

* Find ``HKEY_LOCAL_MACHINE\SYSTEM\Setup`` registry key and create a new key with the name ``LabConfig``;

* Create at least 3 reg DWORD (32-bit) entries with values 1 for compatibility checks to skip during installation.

The following bypass options:

* ``BypassCPUCheck`` – for incompatible CPUs

* ``BypassTPMCheck`` – without a TPM 2.0+ chip

* ``BypassRAMCheck`` – to skip minimum RAM check

* ``BypassSecureBootCheck`` – for Legacy BIOS devices (or UEFI firmware with Secure Boot disabled)

* ``BypassStorageCheck`` – to minimal bypass system drive size check

# 5. Disable GameDVR error popup when playing full screen games:

Powershell or cmd line

* ``reg add HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\GameDVR /f /t REG_DWORD /v "AppCaptureEnabled" /d 0``

* ``reg add HKEY_CURRENT_USER\System\GameConfigStore /f /t REG_DWORD /v "GameDVR_Enabled" /d 0``

# 6. Disable AI/Bing search suggestions in the Start Menu/File Explorer:

* ``REG ADD "HKCU\Software\Microsoft\Windows\CurrentVersion\Search" /V BingSearchEnabled /T REG_DWORD /D 0 /F``

* ``taskkill /f /im explorer.exe``

* ``start explorer.exe``

# 7. Removing bloatware in Powershell without relying on third-party scripts by randoms:

* ``DISM /Online /Get-ProvisionedAppxPackages | select-string Packagename``

# For every type of bloatware on every Windows 11 isntallation the naming will be different, you can make a .ps script for your setup based on this sample:

* ``Remove-ProvisionedAppxPackage -Online -PackageName Clipchamp.Clipchamp_3.0.10220.0_neutral_~_yxz26nhyzhsrt``

# 8. Enabling and installing what you actually need for old games and apps:

.NET 3 5 (2.0) support:

* ``DISM /Online /Enable-Feature /FeatureName:NetFx3 /All``

Directplay:

* ``DISM /Online /Enable-Feature /FeatureName:DirectPlay /All``

# 9. Installing chocolatey package manager via powershell in case winget fails to install what you need:

* ``Set-ExecutionPolicy Bypass -Scope Process -Force;[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072;iex((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))``

# Installing packages system wide that you will need for old games like DirectX/VC Redist browsers and players:

* ``choco install directx vcredist-all vlc mpv firefox``

# 10. Clearing event logs:

* ``for /f "tokens=*" %1 in ('wevtutil.exe el') do wevtutil.exe cl "%1"``

# 11. Listing and deleting Windows shadow copies :

* ``vssadmin list shadows``

* ``vssadmin delete shadows /Shadow={shadow copy ID}``

* ``vssadmin delete shadows /all``

# 12 Disable Windows Updates without gpedit.msc:

* ``net stop "wuauserv"``

* ``sc config "wuauserv" start= disabled``

* ``net stop "UsoSvc"``

* ``sc config "UsoSvc" start= disabled``

* ``net stop "WaaSMedicSvc"``

* ``sc config "WaaSMedicSv" start= disabled``

# 13. Enable Windows Updates without gpedit.msc:

* ``sc config "wuauserv" start= demand``

* ``net start "wuauserv"``

* ``sc config "UsoSvc" start= demand``

* ``net start "UsoSvc"``

* ``sc config "WaaSMedicSvc" start= demand``

* ``net start "WaaSMedicSvc"``

# 14. Fixing most of filesystem errors:

* ``sfc /scannow``

# 15. Cleaning up Windows Update errors and mess:

* ``DISM /online /Cleanup-Image /StartComponentCleanup``

# 16. Other possible troubleshooting:

* ``DISM /Online /Cleanup-Image /ScanHealth``

* ``DISM /Online /Cleanup-Image /CheckHealth``

* ``DISM /Online /Cleanup-Image /RestoreHealth``

# 17. Fix the "Secure Boot CA/keys need to be updated" Event 1801 error in event log:
In Windows Powershell as Admin run
* ``reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Secureboot /v AvailableUpdates /t REG_DWORD /d 0x5944 /f``

Then:

* ``Start-ScheduledTask -TaskName "\Microsoft\Windows\PI\Secure-Boot-Update"``

* Wait for some time 3-5 minutes.

* Go into ``regedit`` and find ``Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecureBoot\Servicing`` the vlaue of ``UEFICA2023Status`` should say ``Updated`` instead of ``In Progress``

* Reboot. Check the ``eventviewer`` for errors again.


# Enjoy and no need for third party scripts.
silentgameplays.
