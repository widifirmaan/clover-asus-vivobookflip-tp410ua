# Clover EFI TP410UA

> Tailored Clover EFI bootloader configuration for running macOS High Sierra, Mojave, and Catalina on the ASUS VivoBook Flip TP410UA.

[![macOS](https://img.shields.io/badge/macOS-10.13_%7C_10.14_%7C_10.15-brightgreen?style=flat-square&logo=apple&logoColor=white)](https://www.apple.com/macos/)
[![Bootloader](https://img.shields.io/badge/Bootloader-Clover_UEFI-blue?style=flat-square&logo=apple&logoColor=white)](https://github.com/CloverHackyColor/CloverBootloader)
[![SMBIOS](https://img.shields.io/badge/SMBIOS-MacBookAir8%2C1-8B5CF6?style=flat-square)](https://everymac.com/)
[![Status](https://img.shields.io/badge/Status-Verified_Stable-success?style=flat-square)](#fitur)

---

## Hardware Target & Status

- **Model**: ASUS VivoBook Flip TP410UA (TP410UAR)
- **Processor**: Intel Core i7-8550U (Kaby Lake Refresh, 4 Cores / 8 Threads, up to 4.0 GHz)
- **Graphics**: Intel UHD Graphics 620 (Full Metal 2 Acceleration, 1536 MB VRAM)
- **Audio Codec**: Realtek ALC256 (Combo Audio Jack & Internal Speakers via AppleALC Layout ID 13/21)
- **Trackpad & Screen**: ELAN/Synaptics I2C Touchpad with Multi-Touch Gestures & FHD IPS Touchscreen
- **Tested macOS Releases**: macOS 10.13 High Sierra, macOS 10.14 Mojave, macOS 10.15 Catalina

---

## Fitur

- **Full Graphics Acceleration**: Native Intel UHD Graphics 620 acceleration with Metal 2 framework support powered by Lilu and WhateverGreen.
- **Crystal Audio & Jack Sensing**: Realtek ALC256 audio configuration with internal stereo speaker output, combo headset jack sensing, and microphone input via AppleALC and CodecCommander.
- **Native Power & Battery Management**: Precise 16-stage battery status monitoring and CPU speed stepping via ACPIBatteryManager and VirtualSMC.
- **I2C Touchpad & Touchscreen**: Smooth macOS multi-touch gestures, palm rejection, and touchscreen input powered by VoodooI2C, VoodooI2CHID, and VoodooI2CELAN.
- **ASUS Hotkeys & Backlight**: Integrated keyboard hotkey handling (brightness control, audio keys) and ambient backlight control via AsusNBFnKeys.
- **Display Brightness & Night Shift**: Hardware PNLF backlight slider support with native macOS Night Shift integration.
- **NVMe & Storage Performance**: Native NVMe controller passthrough for high-throughput system boots and application loading.
- **Broad USB Mapping**: Complete USB port layout mapping via USBInjectAll ensuring high-speed USB 3.0 Type-A and Type-C connectivity.

---

## Tech Stack

| Category | Technology | Description / Purpose |
| :--- | :--- | :--- |
| **Bootloader** | Clover EFI (UEFI Mode) | Modern UEFI bootloader with custom ACPI table injection and patch engine |
| **SMBIOS Profile** | `MacBookAir8,1` | Native power and thermal profile matching Intel Core i7-8550U Kaby Lake-R |
| **ACPI Patches** | Custom Patched DSDT & SSDT | `DSDT.aml`, `SSDT-PNLF.aml`, and EC patches for sleep, brightness, and battery |
| **Core & Graphics** | `Lilu.kext`, `WhateverGreen.kext` | Essential kernel patcher and Intel UHD 620 framebuffer injection |
| **Audio** | `AppleALC.kext`, `CodecCommander.kext` | Native AppleHDA patching for Realtek ALC256 codec |
| **Power & Sensor** | `VirtualSMC.kext`, `ACPIBatteryManager.kext` | Apple SMC emulation and hardware battery gauge integration |
| **Touchpad & Input** | `VoodooI2C.kext`, `VoodooI2CHID.kext`, `VoodooPS2Controller.kext`, `AsusNBFnKeys.kext` | Multi-touch I2C trackpad gestures, keyboard mapping, and Asus Fn keys |
| **Connectivity** | `BT4LEContiunityFixup`, `USBInjectAll`, `GenericUSBXHCI`, `RtWlanU` | Bluetooth BLE continuity patches, USB 3.0 routing, and wireless support |

---

## Project Structure

```bash
/
├── CLOVERX64.efi              # Core Clover EFI bootloader binary
├── config.plist               # Master Clover configuration file (SMBIOS MacBookAir8,1)
├── ACPI/                      # ACPI tables and patches
│   ├── origin/                # Extracted OEM firmware tables
│   └── patched/               # Modified DSDT and SSDT tables (DSDT.aml, SSDT-PNLF.aml)
├── drivers64UEFI/             # 64-bit UEFI drivers (ApfsDriverLoader, AptioMemoryFix, HFSPlus)
├── kexts/Other/               # Essential kernel extensions (Lilu, WhateverGreen, AppleALC, VoodooI2C)
├── themes/                    # Visual bootloader themes (metal, newyear, christmas)
├── tools/                     # Hardware diagnostics and UEFI shell binaries
├── doc/                       # Setup documentation and ACPI patching references
└── README.md
```

---

## Tutorial Run Local & Setup

### 1. Prerequisites

- **Target Laptop**: ASUS VivoBook Flip TP410UA / TP410UAR.
- **USB Flash Drive**: 8 GB or higher formatted as FAT32 / GUID Partition Map.
- **BIOS Configuration**:
  - Disable **Secure Boot** and **Fast Boot**.
  - Set SATA Controller Mode to **AHCI**.
  - Set DVMT Pre-Allocated to **64MB** (or default if locked).

### 2. EFI Deployment

```bash
# 1. Clone the repository
git clone https://github.com/widifirmaan/clover-asus-vivobookflip-tp410ua.git
cd clover-asus-vivobookflip-tp410ua

# 2. Mount your target EFI partition
# macOS:
sudo diskutil mount /dev/diskXs1

# 3. Copy the repository contents into EFI/CLOVER on your EFI partition
mkdir -p /Volumes/EFI/EFI/CLOVER
cp -R * /Volumes/EFI/EFI/CLOVER/
```

### 3. Compatibility Matrix

| macOS Version | Support Status | Architecture Notes |
| :---: | :---: | :--- |
| **High Sierra (10.13.x)** | ✅ Native Support | Full stability with AptioMemoryFix and native graphics acceleration |
| **Mojave (10.14.x)** | ✅ Native Support | Recommended daily driver release with Metal 2 UHD 620 acceleration |
| **Catalina (10.15.x)** | ✅ Native Support | Fully compatible with 64-bit apps and updated VoodooI2C kexts |
| **Big Sur (11.x) & Newer** | ⚠️ Requires OpenCore | Legacy Clover + AptioMemoryFix lacks runtime support for macOS 11+ kernel |

### 4. Troubleshooting Guide

- **Bootloader Hangs at Startup**: Pass `-v` boot argument in Clover Options to view verbose kernel logs.
- **Audio Output Lost After Sleep**: Reset `coreaudiod` service (`sudo killall coreaudiod`) or verify ALC layout ID in `config.plist`.
- **Trackpad Not Responding**: Ensure `VoodooI2C.kext` and `VoodooI2CHID.kext` are loaded in `kexts/Other/` and I2C controller is enabled in BIOS.

---

## Author

Made with ❤️ by **Widi Firmansyah** / [widifirmaan.web.id](https://widifirmaan.web.id)
