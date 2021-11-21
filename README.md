# Hackintosh project | MSI GL6 8RE Laptop

Works with Mac OS Catalina. 10.15.7

OpenCore Version: 0.6.9

## Functionality

- [ ] Battery
- [x] Ethernet
- [ ] Wifi
- [ ] Bluetooth
- [ ] Sound
- [x] USB
- [ ] SD Card port
- [x] Sleep mode
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

## ACPI

For a Coffee Lake processor, we need the following ssdt:

- SSDT-PLUG: cpu => Valid
  path: SB.PR00

- SSDT-USBX: Usb => Valid

- SSDT-PNLF: Backlight => Valid
  Need either a Linux or Windows machine to build this one.
  Use the prebuilt one for now.

- SSDT-PMC: NVRAM => Valid
  LPC (Low Pin Count): LPCB
  PCI: PCI0

- SSDT-RTC0: System Clock => Valid
  We have something when searching for PNP0B00, but we don't have an `sta` method.
  LPC (Low Pin Count): LPCB
  PCI: PCI0

- SSDT-XOSI/SSDT-GPI0 (post-install): i2c Trackpad

- SSDT-AWAC: System Clock
  When searching ACPI000E, we get nothing, this mean that we don't need to do anything for awac.

- SSDT-EC: Embedded Controllers
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
