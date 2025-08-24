# 💻 EFI Hackintosh - OpenCore 1.0.5 (macOS Sequoia 15.6)

![OpenCore](https://img.shields.io/badge/OpenCore-1.0.5-blue?logo=apple)  
![macOS](https://img.shields.io/badge/macOS-Sequoia%2015.6-lightgrey?logo=apple)  
![Status](https://img.shields.io/badge/Boot-Working-brightgreen)  
![Sleep](https://img.shields.io/badge/Sleep-Not%20Working-red)  
![iServices](https://img.shields.io/badge/iServices-Not%20Working-red)  

EFI ini dibuat untuk menjalankan **macOS Sequoia 15.6** menggunakan **OpenCore 1.0.5**.  
This EFI is built to run **macOS Sequoia 15.6** using **OpenCore 1.0.5**.  

---

## 📊 Spesifikasi Hardware / Hardware Specification

| Komponen / Component | Detail                                               |
|----------------------|------------------------------------------------------|
| Laptop/PC            | Lenovo IdeaPad Slim 3 14IML05                        |
| CPU                  | Intel Core i5-10210U (Comet Lake-U)                  |
| iGPU                 | Intel UHD Graphics 620 (✅ QE/CI berfungsi penuh / fully working) |
| dGPU                 | NVIDIA MX (❌ tidak didukung / not supported)         |
| Audio                | Realtek ALC230 (AppleALC.kext)                       |
| Wi-Fi / BT           | Intel AX201 (itlwm & IntelBluetooth)                 |
| Storage              | NVMe SSD                                             |
| Bootloader           | OpenCore 1.0.5                                       |

---

## ✅ Status Fitur / Feature Status

| Fitur / Feature       | Status | Catatan / Notes                                                   |
|-----------------------|--------|-------------------------------------------------------------------|
| Boot macOS Sequoia 15.6 | ✅   | Berjalan lancar / Fully bootable with OpenCore 1.0.5              |
| Intel UHD 620 (QE/CI) | ✅     | Akselerasi grafis penuh / Full graphics acceleration              |
| Audio (ALC230)        | ✅     | AppleALC + Layout-ID                                              |
| Keyboard & Trackpad   | ✅     | VoodooPS2Controller                                               |
| USB Mapping           | ✅     | Stabil / Stable                                                   |
| Battery Status        | ✅     | SMCBatteryManager                                                 |
| HDMI / External Display | ✅   | Berfungsi normal / Working normally                               |
| Wi-Fi Intel AX201     | ✅     | Dengan itlwm.kext / With itlwm.kext                               |
| Bluetooth Intel       | ✅     | Dengan IntelBluetoothFirmware.kext / With IntelBluetoothFirmware.kext |
| Sleep / Wake          | ❌     | Tidak bisa resume / Resume not working (freeze/black screen)      |
| iServices             | ❌     | iMessage/FaceTime/AirDrop belum stabil / Unstable, Broadcom recommended |

---

## 🗂 Struktur Folder EFI / EFI Folder Structure
```
EFI
 ├── BOOT
 └── OC
     ├── ACPI
     ├── Drivers
     ├── Kexts
     ├── Resources
     └── config.plist
```

---

## 🔧 Kext yang Digunakan / Kexts Used
- Lilu.kext  
- WhateverGreen.kext  
- VirtualSMC.kext + SMCBatteryManager, SMCProcessor, SMCLightSensor, SMCSuperIO  
- AppleALC.kext  
- VoodooI2C + PlugIns (VoodooI2CServices, VoodooGPIO, VoodooInput, VoodooI2CHID)  
- VoodooPS2Controller + PlugIns (VoodooPS2Keyboard, VoodooPS2Mouse, VoodooPS2Trackpad)  
- IntelBluetoothFirmware.kext + IntelBTPatcher.kext + BlueToolFixup.kext  
- itlwm.kext / AirportItlwm.kext  
- BrightnessKeys.kext  
- NVMeFix.kext  
- RestrictEvents.kext  

---

## 📑 Ringkasan config.plist / config.plist Summary

### ACPI
- SSDT aktif / Active: `SSDT-PNLF`, `SSDT-AWAC`, `SSDT-EC-USBX-LAPTOP`, `SSDT-OLARILA-MOBILE`, `SSDT-PLUG-DRTNIA`  
- Quirks: `ResetLogoStatus`

### Kernel
- Load order includes: Lilu, VirtualSMC, AppleALC, IOSkywalkFamily, IO80211FamilyLegacy, AirportItlwm, BlueToolFixup, BrightnessKeys, IntelBluetooth, NVMeFix, RestrictEvents, VoodooI2C (+plugins), VoodooPS2 (+plugins), WhateverGreen.  

### NVRAM
- `boot-args`: disesuaikan / adjusted for debugging & patches  
- `csr-active-config`: SIP disabled (umumnya 0xE7030000 / commonly 0xE7030000)  
- `prev-lang:kbd`: default bahasa input / default input language  

### PlatformInfo
- **SMBIOS Generic**:  
  - SystemProductName: `MacBookPro16,3` (Comet Lake-U)  
  - Serial Number / MLB / ROM: **placeholder** → **WAJIB / MUST replace with your own**  

### UEFI
- Drivers aktif / Active: OpenRuntime.efi, HfsPlus.efi, ApfsDriverLoader.efi  
- Quirks: sesuai rekomendasi OpenCore / according to OpenCore recommendations  

### Misc
- Boot: PickerMode, Timeout = 5s, ShowPicker = True  
- Security: SecureBootModel = Default  
- Tools aktif: OpenShell, ResetNvram, etc.  

---

## 🚀 Cara Pakai / How to Use
1. Salin folder **EFI** ke partisi **EFI** USB Installer.  
   Copy the **EFI** folder to the **EFI partition** of your USB installer.  
2. Boot ke macOS installer via OpenCore Picker.  
   Boot into macOS installer via OpenCore Picker.  
3. Setelah instalasi, copy EFI ke SSD/HDD EFI partition.  
   After installation, copy the EFI to your SSD/HDD EFI partition.  
4. Edit `config.plist` dengan **ProperTree/OCAT** untuk ganti serial number, MLB, ROM.  
   Edit `config.plist` with **ProperTree/OCAT** to replace serial number, MLB, ROM.  

---

## 📌 Catatan & Saran / Notes & Tips
- Gunakan SMBIOS `MacBookPro16,3` sebagai dasar.  
  Use SMBIOS `MacBookPro16,3` as baseline.  
- **WAJIB / MUST** ganti **SystemSerialNumber, MLB, UUID, ROM** dengan milik sendiri agar:  
  - iMessage, FaceTime, App Store, iCloud bisa login / can work properly.  
  - Tidak konflik dengan Mac lain / avoid conflicts with other Macs.  
- **Cara Generate SMBIOS / How to generate SMBIOS:**  
  1. Download **GenSMBIOS** dari GitHub.  
     Download **GenSMBIOS** from GitHub.  
  2. Jalankan `GenSMBIOS.command` → pilih `3` (**Generate SMBIOS**) → target `MacBookPro16,3`.  
     Run `GenSMBIOS.command` → choose `3` (**Generate SMBIOS**) → enter `MacBookPro16,3`.  
  3. Salin hasil Serial, MLB, UUID, ROM ke `PlatformInfo -> Generic`.  
     Copy the generated Serial, MLB, UUID, ROM into `PlatformInfo -> Generic`.  
  4. Simpan config.plist, reboot, uji login iCloud/App Store.  
     Save config.plist, reboot, test iCloud/App Store login.  
- Simpan SMBIOS kamu, jangan dibagikan publik.  
  Keep your SMBIOS private, never share it publicly.  
- Untuk Sleep mungkin perlu USB Map custom + patch RTC/EC tambahan.  
  For Sleep you may need custom USB mapping + additional RTC/EC patches.  
- Broadcom card disarankan untuk AirDrop/Handoff/Continuity.  
  Broadcom card recommended for full AirDrop/Handoff/Continuity support.  

---

## 📝 Changelog
**v1.0.5 (Sequoia 15.6)**  
- Release awal / Initial release  
- Semua fitur berjalan kecuali / All features working except **Sleep** and **iServices**  

---

## ⚖️ Disclaimer
EFI ini dibuat untuk tujuan edukasi. Gunakan dengan risiko Anda sendiri.  
This EFI is made for educational purposes only. Use at your own risk.  
Developer tidak bertanggung jawab atas kerusakan akibat penggunaan EFI ini.  
The developer is not responsible for any damage caused by using this EFI.
