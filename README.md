# Hackintosh project | MSI GL6 8RE Laptop

Works with Mac OS Catalina 15.5.7

OpenCore Version: 0.6.9

## Functionality

- [x] USB
- [x] Sleep mode
- [x] Battery
- [x] Ethernet
- [x] Wifi
- [x] Bluetooth
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

### Bios

Thanks to https://github.com/jbwharris/hackintosh-msi-GL72M-7RDX/blob/master/README.md repo.

**Some options only available in advanced mode:**\
In BIOS, holding **ALT + RIGHT-CTRL + SHIFT** together then press **F2**

| Disable |
|--|
| `Fast Boot` |
| `Secure Boot` |
| `Serial/COM Port` |
| `VT-d` |
| `CSM` |
| `Thunderbolt` |
| `Intel SGX` |
| `Intel Plateform Trust` |
| `CFG Lock` |

| Enable |
|--|
| `VT-x` |
| `Above 4G decoding` |
| `Hyper-threading` |
| `Execute Disable Bit` |
| `EHCI/XHCI Hand-off` |
| `OS type: Windows 8.1/10 UEFI Mode` |
| `DVMT Pre-Allocated(iGPU Memory): 64MB` |
| `SATA Mode: AHCI` |

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

### Needed:

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

### Needed:

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
