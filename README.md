# MSI-Z77A-G45-Hackintosh
[GUIDE] Installing macOS Catalina (10.15.x) on MSI Z77A-G45 using Clover UEFI

### Overview

My computer on the base Intel Core i5-3225 with motherboard MSI Z77A-G45. MacOS 10.15.5. All devices work very well.

#### Specs
- **CPU:** Intel Core i3-3225, 3.3 ГГц (dual-core)
- **RAM:** 4x4 Gb DDR3 Samsung 1333 Mhz
- **SSD:** 120 Gb SSD SATA, OSZ Vertex 3
- **GPU:** Intel HD Graphics HD4000
- **Ports:** USB2.0/USB3.0/LAN/HDMI/DVI/3'5 Jack

#### BIOS settings

![frst img](/img/bios.png)

#### Creating USB

**I recommend to create on the USB-flash with USB3.0, because the install will very long.** Creating USB and installing using Clover UEFI works on the MSI Z77A-G45. Make USB flash with GPT parition for Clover UEFI.

Terminal:

> diskutil list

> diskutil partitionDisk /dev/disk1 1 GPT HFS+J "install_osx" R
- EFI will be created automatically.
- Second partition, "install_osx", HFS+J, remainder.

**The plist files in this guide require Clover v4658 or newer. For full functionality and best choice.**

1. Download **UniBeast - Catalina** installer on tonymacx86: https://www.tonymacx86.com/resources/unibeast-10-3-0-catalina.490/
2. Download MacOS Catalina from AppStore.
3. Follow clear USB-flash
4. Run UniBeast:
 - check USB-flash in Destination Select. It will automatically select.
 - check "UEFI Boot Mode" in the Bootloader Configuration.
 - create USB-bootloader
5. Terminal:

> sudo diskutil mount disk1s1

Remove CLOVER from /EFI folder. Download CLOVER from here and copy to /EFI.

#### Createinstallmedia method

This is the same mechanism you would use to create a USB installer for a real Mac Mojave.

It is a single line, executed in Terminal:

> sudo "/Applications/Install macOS Mojave.app/Contents/Resources/createinstallmedia" --volume  /Volumes/install_osx --nointeraction

USB bootloader is ready.

#### Installation

1. Download **UniBeast - Catalina** installer on tonymacx86: https://www.tonymacx86.com/resources/unibeast-10-3-0-catalina.490/
2. Download MacOS Catalina from AppStore.
3. Follow clear USB-flash
4. Run UniBeast:
 - check USB-flash in Destination Select. It will automatically select.
 - check "UEFI Boot Mode" in the Bootloader Configuration.
 - create USB-bootloader
5. Terminal:

> sudo diskutil mount disk1s1

6. Remove CLOVER from /EFI folder. Download CLOVER from here and copy to /EFI.
7. Reboot and Install MacOS Catalina.

#### Post Installation

After installation mount local EFI disk. Terminal:

> sudo diskutil mount disk0s1

Download **MultiBeast - Catalina** installer on tonymacx86: https://www.tonymacx86.com/resources/multibeast-12-3-0-catalina.491/

Run:
- QuickStart - UEFI Boot Mode
- Drivers - RealtekRTL8111 v2.2.2
- Customize: 
  1. Graphic Configuration - Core Graphics Fixup AKA WhateverGreen,
  2. System Definitions - iMac 13.3
- Build -> Install

Replace config.plist from here/EFI/CLOVER to EFI partition (/EFI/CLOVER).

Copy here/EFI/ACPI to /EFI/

Reboot system. MacOS Catalina is ready.

#### Adding: Problem with hibernation ####

Everything required for CPU/IGPU power management is already installed with the steps above.
There is no longer any need to use the ssdtPRgen.sh script.

Be aware that hibernation (suspend to disk or S4 sleep) is not well supported on hackintosh.

You should disable it:
Code:
> sudo pmset -a hibernatemode 0
> sudo rm /var/vm/sleepimage
> sudo mkdir /var/vm/sleepimage

Always check your hibernatemode after updates and disable it. System updates tend to re-enable it, although the trick above (making sleepimage a directory) tends to help.

**UPD 23.03.2020:** MacOS Catalina (10.15.x) is working. Moving from macOS Mojave to macOS Catalina with an existing MultiBeast 11 for macOS Mojave installation. The following directions allow a user to manually remove kexts from /Library/Extensions and recache system on macOS Catalina.

1. Navigate to /Library/Extensions
2. If any 3rd party kexts exist, delete them:

**AHCI_3rdParty_eSATA.kext, AHCI_3rdParty_SATA.kext, AHCI_Intel_Generic_SATA.kext, AppleALC.kext, AppleIGB.kext, AppleIntelE1000e.kext, AtherosE2200Ethernet.kext, FakePCIID_XHCIMux.kext, FakePCIID.kext, GenericUSBXHCI.kext, IntelMausiEthernet.kext, Lilu.kext, NullCPUPowerManagement.kext, RealtekRTL8111.kext, USBInjectAll.kext, VoodooHDA.kext, VoodooTSCSync.kext, WhateverGreen.kext**
 
3.  For reference, the default macOS Catalina /Library/Extensions from a clean installation:
- ACS6x.kext
- ArcMSR.kext
- ATTOCelerityFC8.kext
- ATTOExpressSASHBA2.kext
- ATTOExpressSASRAID2.kext
- CalDigitHDProDrv.kext
- HighPointIOP.kext
- HighPointRR.kext
- PromiseSTEX.kext
- SoftRAID.kext

4. Open /Applications/Utilities/Terminal
5.  > sudo -s
6.  > mount -uw /
7. > touch /Library/Extensions /System/Library/Extensions
8. > kextcache -i /
9. Reboot

**Enjoy it!**
