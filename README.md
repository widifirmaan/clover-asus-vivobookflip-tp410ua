<div align="center">

# 🍎 Clover EFI - ASUS VivoBook Flip TP410UA

**Clover EFI** is a refined, ready-to-use configuration for running **macOS** on the **ASUS VivoBook Flip TP410UA**. This project provides carefully patched ACPI files and essential kernel extensions for daily system stability.

![Status](https://img.shields.io/badge/Status-Working-success?style=for-the-badge)
![Clover](https://img.shields.io/badge/Bootloader-Clover-blue?style=for-the-badge&logo=apple)
![macOS](https://img.shields.io/badge/macOS-Supported-brightgreen?style=for-the-badge&logo=apple)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)

---

</div>

## 📸 Hardware & Configuration Notes
This Clover EFI configuration is tailored specifically for the **ASUS VivoBook Flip TP410UA** (Intel Core i7-8550U, UHD Graphics 620). Comprehensive setup guides and ACPI patching references are available in the [`doc/`](doc/) folder.

---

## 🚀 Features Overview

### ✅ Working Features
*   ✨ **Graphics Acceleration**: Intel UHD 620 fully accelerated with Metal support.
*   🔊 **Audio**: Realtek ALC256 internal speakers and combo audio jack via AppleALC.
*   🔋 **Battery Management**: Accurate status monitoring with ACPIBatteryManager.
*   💾 **Storage**: Full NVMe performance for fast boot and app loading.
*   🖱️ **Trackpad**: Smooth multi-touch gestures with VoodooI2C.
*   ⌨️ **Keyboard**: Fully functional ASUS hotkeys and backlight control.
*   📊 **Display**: PNLF support for brightness control and night shift.
*   🔌 **Connectivity**: All USB ports and HDMI output functional.

### ⚠️ Known Issues
*   **AirDrop**: Currently restricted due to Intel Wireless hardware limitations.

---

## 💻 Hardware Specifications

| Component | Specification |
|:---:|:---|
| **Processor** | Intel Core i7-8550U (Kaby Lake Refresh) |
| **Graphics** | Intel UHD Graphics 620 |
| **RAM** | Up to 16GB DDR4 |
| **Storage** | NVMe SSD Support |
| **Display** | 14" Full HD IPS Touchscreen |
| **Audio** | Realtek ALC256 Codec |
| **Wireless** | Intel WiFi & Bluetooth |
| **Trackpad** | ELAN/Synaptics I2C |

---

## 🛠 Tech Stack

### Bootloader & Core
*   **Bootloader**: Clover Bootloader (UEFI)
*   **SMBIOS Profile**: `MacBookAir8,1` (Tailored for Intel Core i7-8550U Kaby Lake-R)
*   **ACPI**: Custom Patched DSDT/SSDT (`DSDT.aml`, `SSDT-PNLF.aml`)
*   **Core Framework**: Lilu + WhateverGreen for graphics and system fixes.

### Kernel Extensions (Kexts)
*   **Core & Graphics**: `Lilu`, `WhateverGreen`
*   **Audio**: `AppleALC`, `CodecCommander`
*   **SMC & Power**: `VirtualSMC`, `ACPIBatteryManager`
*   **Touchpad, Touchscreen & Input**: `VoodooI2C`, `VoodooI2CHID`, `VoodooI2CELAN`, `VoodooI2CSynaptics`, `VoodooI2CAtmelMXT`, `VoodooI2CFTE`, `VoodooI2CUPDDEngine`, `VoodooPS2Controller`, `AsusNBFnKeys`
*   **Bluetooth & Continuity**: `BT4LEContiunityFixup`
*   **System Fixes & CPU**: `FakePCIID`, `EFICheckDisabler`, `VoodooTSCSync`
*   **USB & Networking**: `USBInjectAll`, `GenericUSBXHCI`, `NullEthernet`, `RtWlanU`, `RtWlanU1827`

---

## 📂 Project Structure

```bash
/
├── CLOVERX64.efi              # Core Clover Bootloader
├── config.plist               # Main configuration file
├── ACPI/                      # ACPI Patches (DSDT/SSDT)
│   ├── origin/                # Original firmware files
│   └── patched/               # Modified/Patched files
├── drivers64UEFI/             # Modern UEFI drivers (APFS, HFS+)
├── kexts/Other/               # Essential Kernel Extensions
├── themes/                    # Visual bootloader themes
└── tools/                     # System utilities (UEFI Shell)
```

---

## 📦 Getting Started

### Prerequisites
*   **Laptop**: ASUS VivoBook Flip TP410UA.
*   **Storage**: USB Drive (8GB+).
*   **Firmware**: Secure Boot Disabled and AHCI Mode enabled.

### 1. Repository Setup
```bash
git clone https://github.com/widifirmaan/clover-asus-vivobookflip-tp410ua.git
cd clover-asus-vivobookflip-tp410ua
```

### 2. Manual Installation
1.  **Mount EFI Partition**: Use `diskutil` (macOS) or Disk Management (Windows/Linux) to mount your EFI partition.
2.  **Deploy Files**: Copy the contents of this repository to the `EFI/CLOVER` directory on your EFI partition.
3.  **Bios Config**: Disable Secure Boot, Fast Boot, and set SATA mode to AHCI.
4.  **Reboot**: Select Clover as your primary boot option in BIOS.

---

## 🔧 Troubleshooting

### Boot Issues
*   **Bootloader tidak muncul**: Periksa apakah file EFI sudah tersalin dengan benar, verifikasi urutan boot di BIOS, atau coba reset NVRAM/PRAM.
*   **Verbose Mode**: Use `-v` boot argument to see the boot log if it hangs.
*   **Safe Mode**: Use `-x` for minimal driver loading.
*   **Kernel Panic**: Usually related to incompatible kexts or SMBIOS. Lihat log dengan `-v`, periksa versi kext, atau gunakan Safe Boot.

### Feature Fixes
*   **Audio**: If audio is lost, try resetting `coreaudiod` (e.g., `killall coreaudiod`) or check the Layout ID in `config.plist`.
*   **Trackpad**: Ensure VoodooI2C kexts are loaded and the trackpad is not disabled in BIOS.
*   **Baterai**: Pastikan `ACPIBatteryManager.kext` aktif dan periksa `SSDT-EC` di folder ACPI.

---

## 📊 Compatibility Matrix

| macOS Version | Status | Notes |
|:---:|:---:|:---|
| High Sierra (10.13.x) | ✅ Supported | Native support with AptioMemoryFix |
| Mojave (10.14.x) | ✅ Supported | Full Metal acceleration on UHD 620 |
| Catalina (10.15.x) | ✅ Supported | High stability for daily development |
| Big Sur (11.x) & Newer | ⚠️ Needs OpenCore | Legacy Clover + AptioMemoryFix cannot boot macOS 11+; OpenCore with OpenRuntime required |

---

## 👥 Authors
Developed and maintained by **Widi Firmaan**.
Special thanks to the **Hackintosh Community** for the continuous support.

---

## ⚠️ Disclaimer
*   This project is for educational and research purposes only.
*   Running macOS on non-Apple hardware may violate Apple's TOS.
*   The author is not responsible for any hardware damage or data loss.
*   **Always backup your data before proceeding.**

---

## 📄 License
This configuration is released for educational and personal use under the **MIT License**.
