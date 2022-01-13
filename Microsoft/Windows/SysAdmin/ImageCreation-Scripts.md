# Cheatsheets • Microsoft • Windows • SysAdmin • Image Creation • Script Reference
A list of all the various ways to create, capture, and deploy Windows 10 and Windows 11 using CMD commands, Batch files `(.bat)`, PowerShell commands and PowerShell scripts `(.ps1)`. Please note that you would likely only use one of the commands or scripts from each section below, this list is simply a consolidation of all the different methods I've found throughout either Microsoft's official documentation or via a blog post, Reddit thread, etc. I'll do my best to provide the source for each.

---

## Windows Preinstallation Environment (WinPE)
> [Microsoft Docs • WinPE • Mount and Customize](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-mount-and-customize?view=windows-11)

### Add Device Drivers `(.inf)` to WinPE
```bat
Dism /Add-Driver /Image:"C:\WinPE_amd64\mount" /Driver:"C:\SampleDriver\driver.inf"
```

### Add Startup Scripts to WinPE
```bat
Modify "C:\WinPE_amd64\mount\Windows\System32\Startnet.cmd" to include customized commands.
```

### Add Temporary Storage (Scratch Space) to WinPE
```bat
Dism /Set-ScratchSpace:256 /Image:"C:\WinPE_amd64\mount"
```

### Set the Power Plan to High Performance
```bat
wpeinit
powercfg /s 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c
```

### Add `PowerShell` Support to WinPE 

> [Source: Microsoft Docs](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-adding-powershell-support-to-windows-pe?view=windows-11)

```bat
@ECHO OFF
rem Step 1 - Mount the Image
echo Mounting the WinPE image, please wait...
Dism /Mount-Image /ImageFile:"C:\WinPE_amd64_PS\media\sources\boot.wim" /Index:1 /MountDir:"C:\WinPE_amd64_PS\mount"

rem Step 2 - Add the Required Packages
echo Done!
echo Installing the packages required for PowerShell, please wait...
Dism /Add-Package /Image:"C:\WinPE_amd64_PS\mount" /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\WinPE-WMI.cab"
Dism /Add-Package /Image:"C:\WinPE_amd64_PS\mount" /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\en-us\WinPE-WMI_en-us.cab"
Dism /Add-Package /Image:"C:\WinPE_amd64_PS\mount" /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\WinPE-NetFX.cab"
Dism /Add-Package /Image:"C:\WinPE_amd64_PS\mount" /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\en-us\WinPE-NetFX_en-us.cab"
Dism /Add-Package /Image:"C:\WinPE_amd64_PS\mount" /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\WinPE-Scripting.cab"
Dism /Add-Package /Image:"C:\WinPE_amd64_PS\mount" /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\en-us\WinPE-Scripting_en-us.cab"
Dism /Add-Package /Image:"C:\WinPE_amd64_PS\mount" /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\WinPE-PowerShell.cab"
Dism /Add-Package /Image:"C:\WinPE_amd64_PS\mount" /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\en-us\WinPE-PowerShell_en-us.cab"
Dism /Add-Package /Image:"C:\WinPE_amd64_PS\mount" /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\WinPE-StorageWMI.cab"
Dism /Add-Package /Image:"C:\WinPE_amd64_PS\mount" /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\en-us\WinPE-StorageWMI_en-us.cab"
Dism /Add-Package /Image:"C:\WinPE_amd64_PS\mount" /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\WinPE-DismCmdlets.cab"
Dism /Add-Package /Image:"C:\WinPE_amd64_PS\mount" /PackagePath:"C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\en-us\WinPE-DismCmdlets_en-us.cab"

rem Step 3 - Unmount the Image and Commit Changes
echo Done!
echo Committing changes to the image then unmounting, please wait...
Dism /Unmount-Image /MountDir:C:\WinPE_amd64_PS\mount /Commit
echo Done!
pause
```

### Create a `.ISO` from WinPE Source Files
> [Source: Microsoft Docs](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/winpe-create-usb-bootable-drive?view=windows-11#create-a-winpe-iso-dvd-or-cd)
```cmd
MakeWinPEMedia /ISO C:\WinPE_amd64 C:\WinPE_amd64\WinPE_amd64.iso
```

---

## Customize
These scripts are used to customize the Windows 10/11 image you're going to capture and deploy.

### Install a Downloaded Windows Update (KB) `.msu`
```bat
Dism /Add-Package /Image:"C:\WinPE_amd64\mount" /PackagePath:"E:\windows10.0-kbxxxxx.msu"
```

---

## Capture

### Capture an Image (.wim) using DISM from Command Prompt in WinPE
> [Source: Microsoft Docs](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/deploy-a-custom-image?view=windows-11#step-3-capture-an-image-of-the-installation)
```bat
Dism /Capture-Image /ImageFile:C:\myimage.wim /CaptureDir:c:\ /Compress:fast /CheckIntegrity /Name:"Home" /Description:"Home reference installation"
```
### Transfer Captured Image `(.wim)` to a Network Share
Once capture is complete, transfer to a network share and overwrite the source/original `.wim` from the extracted `.ISO`.
```bat
net use N: \\server\share\
copy C:\myimage.wim N:\WindowsDVD\sources\install.wim
```

---

## Deploy
