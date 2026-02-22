<div align="center">
  
# 🍎 Clover EFI Hackintosh - ASUS VivoBook Flip TP410UA

[![Clover](https://img.shields.io/badge/Bootloader-Clover-blue?style=for-the-badge&logo=apple)](https://sourceforge.net/projects/cloverefiboot/)
[![macOS](https://img.shields.io/badge/macOS-Supported-brightgreen?style=for-the-badge&logo=apple)](https://www.apple.com/macos/)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Working-success?style=for-the-badge)](https://github.com)

**Konfigurasi Clover EFI siap pakai untuk menjalankan macOS di laptop ASUS VivoBook Flip TP410UA**

⚠️ **Catatan:** Semua fitur berfungsi dengan baik kecuali AirDrop

</div>

---

## 💻 Spesifikasi Hardware

**ASUS VivoBook Flip TP410UA**

| Komponen | Spesifikasi |
|----------|------------|
| **Processor** | Intel Core i7-8550U (Kaby Lake Refresh) |
| **Graphics** | Intel UHD Graphics 620 |
| **RAM** | Hingga 16GB DDR4 |
| **Storage** | NVMe SSD |
| **Display** | 14" Full HD IPS Touchscreen |
| **Audio** | Realtek Audio Codec |
| **Wireless** | Intel WiFi & Bluetooth |
| **Trackpad** | ELAN/Synaptics I2C |

---

## ✅ Fitur yang Didukung

- ✨ **Graphics Acceleration** - Intel UHD 620 dipercepat dengan Metal
- 🔊 **Audio** - Realtek ALC256 via AppleALC
- 🔋 **Battery Management** - ACPIBatteryManager
- 💾 **Storage** - Full NVMe support
- 🖱️ **Trackpad** - VoodooI2C dengan dukungan gesture
- ⌨️ **Keyboard** - ASUS Function Keys
- 🔌 **USB** - Semua port USB functional
- 🌐 **Ethernet** - Via USB (jika diperlukan)
- 🔒 **Secure Boot** - Compatible
- 🎨 **Display Brightness Control** - PNLF support

⚠️ **Tidak Didukung:**
- AirDrop (Intel Wireless limitation)

---

## 📦 Prasyarat Instalasi

### Persyaratan

- Laptop ASUS VivoBook Flip TP410UA
- USB Drive minimum 8GB
- Akses ke macOS atau aplikasi pembuat bootable
- Administrator/root access
- EFI firmware yang sudah di-unlock

### Tools yang Diperlukan

- **balenaEtcher** atau **Etcher** untuk membuat USB bootable
- **UEFI Shell** (sudah included dalam konfigurasi ini)
- **Clover Bootloader** (sudah included)

---

## 🚀 Panduan Instalasi

### Langkah 1: Persiapan

```bash
# Clone repository
git clone https://github.com/yourusername/clover-asus-vivobookflip-tp410ua.git
cd clover-asus-vivobookflip-tp410ua
```

### Langkah 2: Salin ke Partisi EFI

1. **Identifikasi Partisi EFI:**
   ```bash
   diskutil list
   ```

2. **Mount Partisi EFI:**
   ```bash
   # macOS
   sudo mkdir -p /Volumes/EFI
   sudo mount -t msdos /dev/disk0s1 /Volumes/EFI
   ```

3. **Salin File Clover:**
   ```bash
   # Backup konfigurasi lama (opsional)
   sudo cp -r /Volumes/EFI/EFI /Volumes/EFI/EFI.backup
   
   # Salin file baru
   sudo cp -r EFI /Volumes/EFI/
   ```

4. **Unmount Partisi:**
   ```bash
   sudo umount /Volumes/EFI
   ```

### Langkah 3: Konfigurasi BIOS

1. Restart dan masuk ke BIOS (F2 atau Delete)
2. Disable Secure Boot
3. Enable AHCI mode untuk Storage
4. Set UEFI boot mode
5. Save dan Exit

### Langkah 4: Boot macOS

1. Restart dengan USB bootable macOS
2. Pilih "Clover Bootloader" di menu
3. Pilih partisi macOS
4. Install atau Boot sesuai kebutuhan

---

## 📁 Struktur File Konfigurasi

```
├── CLOVERX64.efi              # Bootloader Clover utama
├── config.plist               # Konfigurasi Clover
│
├── ACPI/
│   ├── origin/                # DSDT/SSDT original dari firmware
│   └── patched/               # DSDT/SSDT yang sudah dimodifikasi
│
├── drivers64/                 # Legacy drivers (BIOS mode)
├── drivers64UEFI/             # UEFI drivers modern
│   ├── ApfsDriverLoader       # APFS filesystem support
│   ├── HFSPlus                # HFS+ filesystem support
│   ├── AptioMemoryFix         # Memory allocation fix
│   └── ...
│
├── kexts/Other/               # Kernel Extensions (drivers)
│   ├── VoodooI2C              # I2C touchpad/trackpad
│   ├── AppleALC               # Audio codec
│   ├── WhateverGreen          # Graphics fixes
│   ├── Lilu                   # Kext patcher
│   ├── VirtualSMC             # System Management Controller
│   ├── USBInjectAll           # USB port mapping
│   ├── ACPIBatteryManager     # Battery status
│   └── ...
│
├── themes/                    # Boot themes
│   ├── BGM/                   # BGM theme (default)
│   ├── cesium/
│   └── ...
│
└── tools/                     # Utility tools
    ├── Shell64.efi            # UEFI Shell
    └── bdmesg.efi             # Boot debug messages
```

---

## 🔧 Troubleshooting

### Masalah Boot

**Bootloader tidak muncul**
- Periksa apakah file EFI sudah tersalin dengan benar
- Verifikasi urutan boot di BIOS
- Coba reset NVRAM/PRAM

**Kernel Panic**
- Lihat log dengan boot argument: `-v` (verbose)
- Periksa compatibility kext versions
- Gunakan Safe Boot: `-x` flag

### Audio Tidak Bekerja

```bash
# Reset audio codec
defaults delete com.apple.AppleHDA
killall coreaudiod
```

### Trackpad Tidak Merespons

- Verifikasi VoodooI2C kext terinstall
- Update ke versi kext terbaru

### Baterai Tidak Terdeteksi

- Pastikan ACPIBatteryManager.kext aktif
- Periksa SSDT-EC di ACPI folder
- Update config.plist dengan info baterai terbaru

---

## 📊 Kompatibilitas Versi macOS

| Versi macOS | Status | Catatan |
|-----------|--------|---------|
| Big Sur (11.x) | ✅ Tested | Working well |
| Monterey (12.x) | ✅ Tested | Recommended |
| Ventura (13.x) | ⚠️ Experimental | Some issues reported |
| Sonoma (14.x) | ❌ Not Tested | Older hardware |

---

## 📝 File Penting

### `config.plist`

File konfigurasi utama Clover yang berisi:
- Device properties
- Boot arguments
- SMBIOS information
- Kext injection patches
- UEFI/Legacy boot settings

**Backup file ini sebelum melakukan modifikasi!**

### ACPI Files

```
DSDT.aml         - Main ACPI descriptor
SSDT-PNLF.aml    - Panel brightness control
ssdt-rmne.aml    - Memory access optimization
ssdt.aml         - Custom ACPI modifications
```

---

## 🛠️ Tools Bawaan

| Tool | Fungsi |
|------|--------|
| **Shell64.efi** | UEFI command shell untuk debugging |
| **bdmesg.efi** | Tampilkan boot messages |
| **Clover GUI** | Boot menu visual Clover |

---

## 📚 Referensi & Resources

- 🌐 [Clover Official](https://sourceforge.net/projects/cloverefiboot/)
- 📖 [OpenCore Documentation](https://dortania.github.io/OpenCore-Install-Guide/)
- 💬 [Hackintosh subreddit](https://www.reddit.com/r/hackintosh/)
- 🔧 [Insanelymac Forum](https://www.insanelymac.com/)
- 📱 [VivoBook Forum](https://www.asus.com/support/)

---

## 🤝 Kontribusi

Kontribusi sangat diterima! Jika Anda menemukan cara untuk meningkatkan konfigurasi ini:

1. Fork repository ini
2. Buat branch fitur baru (`git checkout -b feature/improvement`)
3. Commit perubahan (`git commit -m 'Add improvement'`)
4. Push ke branch (`git push origin feature/improvement`)
5. Buka Pull Request

---

## ⚠️ Disclaimer

- Proyek ini untuk tujuan edukasi dan penelitian
- Instalasi macOS via Hackintosh dapat melanggar Terms of Service Apple
- Penulis tidak bertanggung jawab atas kerusakan hardware atau data loss
- Backup data penting sebelum memulai proses instalasi
- Gunakan dengan risiko Anda sendiri

---

## 📄 Lisensi

Proyek ini dilisensikan di bawah MIT License - lihat file [LICENSE](LICENSE) untuk detail.

---

<div align="center">

### 💡 Tips & Tricks

**Disable verbose boot:**
Ubah boot argument dari `-v` ke `-v -f` di Clover untuk fresh cache

**Update kext:**
Selalu gunakan versi kext terbaru yang compatible dengan OS versi Anda

**Backup reguler:**
Jangan lupa backup folder EFI sebelum update macOS major

---

**Dibuat dengan ❤️ untuk Hackintosh Community**

Jika repository ini membantu Anda, please ⭐ star repository ini!

Terakhir diupdate: Februari 2026

</div>
