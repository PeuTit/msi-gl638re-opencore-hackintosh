# Hackintosh project | MSI GL6 8RE Laptop

Works with Mac OS Monterey 12.0.1

OpenCore Version: 0.7.5

## Functionality

- [x] USB
- [x] Sleep mode
- [x] Battery
- [ ] Ethernet
- [ ] Wifi
- [ ] Bluetooth
- [ ] Sound
  - [ ] internal speaker
  - [ ] internal microphone
  - [x] jack headset
  - [x] jack microphone
- [ ] SD Card port
- [ ] DisplayPort / Hdmi

## Config

- CPU: Intel Core i5 8300H
- GPU0: Intel UHD Graphics 630
- GPU1: Nvidia Geforce GTX 1060
- SSD: Kingston RBUSNS8154032566J
- ETHERNET:
  - Wireless: Wireless AC 9462
  - Ethernet: Qualcomm Atheros AR8171/8175 PCI-E Gigabit Ethernet Controller NDIS 630
- BLUETOOTH: Intel Wireless Bluetooth
- SOUND CARD: To define

## OpenCore Config.plist

In `config.plist`:

In the Kernel/Add:

Comment=RMII2C.kext
Enabled=False
BundlePath=VoodooRMI.kext/Contents/PlugIns/RMII2C.kext

Comment=VoodooInput.kext
Enabled=False
BundlePath=VoodooRMI.kext/Contents/PlugIns/VoodooInput.kext

## ACPI

For a Coffee Lake processor, we need the following ssdt:

- SSDT-USBX: Usb => Valid

- SSDT-PLUG: cpu => Valid
  path: SB.PR00

- SSDT-PNLF: Backlight => Valid
  Need either a Linux or Windows machine to build this one.
  Use the prebuilt one for now.

- SSDT-PMC: NVRAM => Valid
  LPC (Low Pin Count): LPCB
  PCI: PCI0

- SSDT-USB-Reset: Make the usb port work => Valid

- SSDT-SBUS-MCHC: SMBus (post-install) => Valid
  IOReg name: /PCI0@0/SBUS@1F,4 -> PCI0.SBUS

- SSDT-XOSI/SSDT-GPI0 (post-install): i2c Trackpad

- SSDT-RHUB: Usb => Don't need
  path: PCI0.XHC.RHUB

- SSDT-EC-USBX-LAPTOP: Usb port power => Don't need

- SSDT-AWAC: System Clock => Don't need
  When searching ACPI000E, we get nothing, this mean that we don't need to do anything for awac.

- SSDT-RTC0: System Clock => Don't need
  We have something when searching for PNP0B00, but we don't have an `sta` method.
  LPC (Low Pin Count): LPCB
  PCI: PCI0

- SSDT-EC: Embedded Controllers => Don't need
  Embedded controller name: EC
  Path of our embedded controller: PCI0.LPCB
  Because pnp0c09 is already named EC we don't need an SSDT-EC

Compiling & Decompiling ssdts: https://dortania.github.io/Getting-Started-With-ACPI/Manual/compile.html

## Ressources

- OpenCore guides: https://dortania.github.io/OpenCore-Install-Guide/
- How to create bootable usb with macOS: https://support.apple.com/en-us/HT201372
- MountEfi: https://github.com/corpnewt/MountEFI
- Efi-Agent: https://github.com/headkaze/EFI-Agent
- OpenCore Sanity Checker: https://opencore.slowgeek.com
- OpenCore GenX: https://github.com/Pavo-IM/OC-Gen-X
- IORegistreryClone: https://github.com/khronokernel/IORegistryClone
