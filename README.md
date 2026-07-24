# ASUS TUF Gaming F15 FX506LH Hackintosh (macOS Sequoia)

OpenCore EFI configuration for ASUS TUF Gaming F15 FX506LH running stable macOS Sequoia.

## Hardware Specifications

* **CPU:** Intel® Core™ i5-10300H (Comet Lake)
* **iGPU:** Intel® UHD Graphics 630
* **dGPU:** NVIDIA® GeForce® GTX 1650 (Disabled via boot-args/SSDT)
* **RAM:** DDR4 2933MHz
* **Audio:** Realtek ALC256
* **Ethernet:** Realtek RTL8111
* **Wi-Fi/Bluetooth:** Broadcom BCM94360NG (via M.2 NGFF Adapter)

---

## What's Working 🟢

* QE/CI Graphics Acceleration (Intel UHD 630)
* CPU Power Management & Thermal Control
* Sleep, Shutdown, and Restart
* Battery Indicator & Status
* Internal Speakers, Headphones Audio & Microphone
* Keyboard (with Brightness & Volume Hotkeys)
* Trackpad (I2C Mode with multi-gestures)
* All USB Ports (USB 2.0 & USB 3.0 mapped)
* Built-in Web Camera
* Apple Continuity Features (Native AirDrop, Handoff, Bluetooth, and Wi-Fi)
* iServices (iCloud, App Store, iMessage)

---

## What's Not Working / Known Issues 🔴

* **NVIDIA dGPU GTX 1650:** Not supported by macOS architecture.
* **HDMI Output:** Routed directly through the NVIDIA dGPU, so external display via HDMI does not work.
* **iPhone Mirroring:** Hard-coded hardware limitation. Feature requires Apple Silicon or a physical Apple T2 Security Chip for device attestation.

---

## Broadcom Wi-Fi Post-Installation (Crucial) ⚠️

Since macOS Sonoma/Sequoia dropped native support for Broadcom cards, this EFI requires a specific injection method using **OpenCore Legacy Patcher (OCLP)**.

### 1. Pre-Requisites Applied in Config.plist
Before running OCLP, the following deep modifications have been made to this EFI to allow system-volume patching:
* `IOSkywalkFamily.kext` is blocked under `Kernel -> Block` to stop native Wi-Fi drivers from loading.
* `AMFIPass.kext`, `IOSkywalk.kext`, and `IO80211FamilyLegacy.kext` are injected into `Kernel -> Add` (in strict order).
* SIP (System Integrity Protection) is partially disabled by setting `csr-active-config` to `03080000` or `FF0F0000` under NVRAM.
* `SecureBootModel` is set to `Disabled` under `Misc -> Security`.

### 2. Post-Boot Action
1. Boot into macOS Sequoia using this EFI.
2. Download and open the latest **OpenCore Legacy Patcher** app.
3. Click **Post-Install Root Patch** (The "Modern Wireless" patch button will now be active).
4. Let the patcher download the Kernel Debug Kit (KDK) and inject the wireless drivers into the system volume.
5. Reboot your laptop and perform an NVRAM Reset.

### 3. Video Tutorial Guide 📺
For a detailed step-by-step visual on how this injection rules work, you can follow this excellent video guide by Hendra Hry:

[![Watch Video Guide](https://img.youtube.com/vi/YOUR_VIDEO_ID/maxresdefault.jpg)](https://youtu.be/tBcxSDWM0M0?si=vEwe9NDBtkK-v4Ic)


---

## SMBIOS Info

* **Current SMBIOS Used:** `MacBookPro16,1` or `MacBookPro16,3` (Highly recommended for optimal laptop power management and battery life).
* **Tested Alternatives:** `MacMini8,1` and `iMac20,1` (Both boot successfully but MacBookPro SMBIOS yields better thermal and battery results).

---

## Screenshots 📸

![About This Mac]<img width="1080" height="954" alt="About This Mac" src="https://github.com/user-attachments/assets/f913e950-f7aa-4ef9-962a-6cc18b3f7215" />

![AirDrop and WiFi]<img width="942" height="1252" alt="airdrop" src="https://github.com/user-attachments/assets/602e1a38-e6c3-4940-b25a-8b050c987f8f" />

---

## Important Note Before Using ⚠️

* **DO NOT USE THIS EFI DIRECTLY.**
* You **must** generate your own unique SMBIOS details (`SystemSerialNumber`, `SystemUUID`, `MLB`, `ROM`) using GenSMBIOS or ProperTree before logging into your Apple ID/iCloud to prevent account suspension.

---

## Disclaimer ⚖️

* This project is strictly for **educational, research, and non-profit personal use only**.
* This repository does not host, distribute, or share any copyrighted Apple operating system files or macOS installers.
* This project is not affiliated, authorized, or endorsed by Apple Inc. macOS and Apple are registered trademarks of Apple Inc.
* Use this EFI configuration at your own risk. The author is not responsible for any hardware damage, data loss, bricked systems, or Apple account bans that may occur from using these files.
