# Clover EFI TP410UA

Clover UEFI bootloader configuration, patched ACPI tables, and kernel extensions for running macOS High Sierra, Mojave, and Catalina on the ASUS VivoBook Flip TP410UA.

![Clover](https://img.shields.io/badge/Clover-UEFI-2563EB?style=flat-square&logo=apple&logoColor=white)
![macOS](https://img.shields.io/badge/macOS-10.13_%7C_10.14_%7C_10.15-16A34A?style=flat-square&logo=apple&logoColor=white)
![SMBIOS](https://img.shields.io/badge/SMBIOS-MacBookAir8%2C1-7C3AED?style=flat-square)

## Features

- **Hardware Target (`i3-7100U` / Kaby Lake)**: Configured for the ASUS VivoBook Flip TP410UA (`ACPI/patched/ssdt.aml` targeting `Intel Core i3-7100U @ 2.40GHz`, Intel HD Graphics 620 with `ig-platform-id: 0x59160009` and `PciRoot(0)/Pci(0x02,0)` properties injection).
- **Graphics & Display Backlight**: `Lilu.kext`, `WhateverGreen.kext`, `SSDT-PNLF.aml`, and `AppleBacklight` kext patch for Metal graphics acceleration and macOS brightness control.
- **Audio Support**: `AppleALC.kext` (`Layout ID 12`, `ResetHDA: true`) paired with `CodecCommander.kext` plus DSDT renames (`change HDAS to HDEF`, `change AZAL to HDEF`).
- **I2C Trackpad, Touchscreen & Input**: `VoodooI2C.kext` with satellite plugins (`VoodooI2CHID`, `VoodooI2CELAN`, `VoodooI2CSynaptics`, `VoodooI2CAtmelMXT`, `VoodooI2CFTE`, `VoodooI2CUPDDEngine`), `VoodooPS2Controller.kext`, and `AsusNBFnKeys.kext`.
- **Power, Sensors & System Fixes**: `VirtualSMC.kext`, `ACPIBatteryManager.kext`, `VoodooTSCSync.kext`, custom `DSDT.aml` + `ssdt.aml`, and `Enable TRIM for SSD` kernel patch (`Boot.HibernationFixup = true`, `CsrActiveConfig = 0x67`).
- **USB & Networking**: `USBInjectAll.kext` with `USB 10.13.6 by PMHeart` port-limit patch, `GenericUSBXHCI.kext`, `BT4LEContiunityFixup.kext`, `RtWlanU.kext`, `RtWlanU1827.kext`, and `NullEthernet.kext` + `ssdt-rmne.aml`.
- **Clover Theme**: Configured in `config.plist` (`GUI.Theme = Olarila`, `1920x1080`) with `9` included theme directories (`BGM`, `cesium`, `christmas`, `Clovy`, `embedded`, `metal`, `newyear`, `Olarila`, `random`).

## Tech Stack

| Component | Files / Configuration | Role |
| :--- | :--- | :--- |
| **Bootloader & SMBIOS** | `CLOVERX64.efi`, `config.plist` (`MacBookAir8,1`) | UEFI boot orchestration and macOS SMBIOS identity |
| **ACPI Patched Tables** | `ACPI/patched/` (`DSDT.aml`, `ssdt.aml`, `SSDT-PNLF.aml`, `ssdt-rmne.aml`) | CPU power states, backlight (`PNLF`), EC/device renames, null ethernet |
| **UEFI Drivers** | `drivers64UEFI/` (`12` `.efi` drivers including `ApfsDriverLoader-64`, `AptioMemoryFix-64`, `HFSPlus-64`), `drivers64/` (`4` `.efi` drivers) | APFS/HFS+ filesystem loading and UEFI memory allocation |
| **Core & Graphics Kexts** | `Lilu.kext`, `WhateverGreen.kext`, `FakePCIID.kext`, `EFICheckDisabler.kext` | Runtime kext patching and Intel HD 620 framebuffer injection |
| **Audio & Input Kexts** | `AppleALC.kext`, `CodecCommander.kext`, `VoodooI2C*.kext` (`7` kexts), `VoodooPS2Controller.kext`, `AsusNBFnKeys.kext` | Audio codec layout injection (`12`) and I2C/PS2 input handling |
| **Power, USB & Network Kexts** | `VirtualSMC.kext`, `ACPIBatteryManager.kext`, `VoodooTSCSync.kext`, `USBInjectAll.kext`, `GenericUSBXHCI.kext`, `BT4LEContiunityFixup.kext`, `NullEthernet.kext`, `RtWlanU*.kext` | SMC emulation, battery readout, USB routing, and Wi-Fi/BT support |
| **EFI Utilities & Docs** | `tools/` (`bdmesg.efi`, UEFI Shells), `doc/`, `themes/` | Clover diagnostics, installation guides, and boot GUI themes |

## Local Setup

### Prerequisites

- **Target Hardware**: ASUS VivoBook Flip TP410UA (or compatible Intel Kaby Lake laptop with verified device IDs)
- **USB Flash Drive**: 8 GB+ formatted as FAT32 with GUID Partition Map
- **BIOS Settings**: Disable Secure Boot and Fast Boot; set SATA Mode to `AHCI`
- **No database or application server required** (static EFI bootloader configuration)

### Quick Start

Copy the contents of this repository directly into your mounted EFI partition's `/Volumes/EFI/EFI/CLOVER` directory (do not nest `CLOVER/` twice):

```bash
git clone https://github.com/widifirmaan/clover-asus-vivobookflip-tp410ua.git
cd clover-asus-vivobookflip-tp410ua

# macOS: identify and mount EFI partition (replace diskXs1 with your target disk)
diskutil list
sudo diskutil mount /dev/diskXs1

# Copy repository files into EFI/CLOVER/
mkdir -p /Volumes/EFI/EFI/CLOVER
cp -R ./* /Volumes/EFI/EFI/CLOVER/
```

Validate `config.plist` XML structure locally on macOS or Linux before copying:

```bash
# macOS
plutil -lint config.plist

# Linux / Python 3
python3 -c "import plistlib; plistlib.load(open('config.plist', 'rb')); print('config.plist valid')"
```

Supported target releases for this Clover configuration are macOS High Sierra (`10.13.x`), Mojave (`10.14.x`), and Catalina (`10.15.x`); macOS Big Sur (`11.x`) and newer generally require OpenCore migration. Pass `-v` in Clover boot options for verbose kernel troubleshooting.

## Author

**Widi Firmansyah** · [widifirmaan.web.id](https://widifirmaan.web.id)
