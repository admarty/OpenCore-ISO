## About

A properly configured OpenCore **DVD/CD-format ISO file** for Proxmox VE to create macOS virtual machines.

Supports all Intel-based versions of macOS, from **Mac OS X 10.4** to **macOS 26**.

**For AMD users**:

> Enjoy a true **vanilla macOS** experience with no kernel patches for stable operation.
> This is probably the best way to run macOS on AMD hardware, at least you still retain access to the hypervisor to run other VMs.

---

## 📦 Download

Grab the latest OpenCore ISO and macOS Recovery here:
👉 [Release page](https://github.com/LongQT-sea/OpenCore-ISO/releases)

---

## ⚡ Quick Start Guide

### 1. Create a New VM in the Proxmox VE web interface

---

### 2. General

* **VM ID**: Any available ID
* **Name**: Any name you like for the macOS VM

---

### 3. OS

* **ISO Image**: Select `LongQT-OpenCore-v0.X.iso`
* **Guest OS Type**: Leave as default (`Linux`)

---

### 4. System

* **Machine Type**: `q35`
* **BIOS**: UEFI (OVMF)
* **Add EFI Disk**: ✅ Enabled
* **Pre-Enroll Keys**: ❌ **Untick to disable Secure Boot**
* **QEMU Guest Agent**:

  * ✅ Enable for macOS 10.14 – macOS 26
  * ❌ Leave disabled for macOS 10.4 – macOS 10.13

---

### 5. Hard Disk

The disk **bus type** depends on your needs:

* **VirtIO**: Better performance
* **SATA**: Supports TRIM/discard for more efficient storage usage

| macOS Version            | Support Bus Type        |
| ------------------------ | ----------------------- |
| macOS 10.15 – macOS 26   | `VirtIO Block` / `SATA` |
| macOS 10.4 – macOS 10.14 | `SATA`                  |

**Note:** SATA is recommended because it supports TRIM/discard for more efficient storage usage.

---

### 6. CPU

#### Cores

Choose based on your hardware: 1 / 2 / 4 / 8 / 16 / 32

#### Type (Model):

| macOS Version            | Recommended CPU Type                                                  |
| ------------------------ | --------------------------------------------------------------------- |
| macOS 10.11 – macOS 26   | `Broadwell-noTSX`, `Skylake-Client-v4`, `Skylake-Server-v4` (AVX-512) |
| macOS 10.4 – macOS 10.10 | `Penryn`                                                              |

> ⚠️ **Notes for AMD CPUs**: 
- Tick ✅ Advanced, under **Extra CPU Flags**, turn off `pcid` and `spec-ctrl`.
- For macOS 13–26, instead of using the GUI, you need to set the CPU manually using the qm command, e.g.:
- `qm set [VMID] --args "-cpu Broadwell-noTSX,vendor=GenuineIntel"`
- `qm set [VMID] --args "-cpu Skylake-Client-v4,vendor=GenuineIntel"`

> ⚠️ **Notes for Intel CPUs**:  
> - Avoid using [`host` or `max`](https://browser.geekbench.com/v6/cpu/14313138) CPU types — these can be **~30% slower** (single-core) and **~44% slower** (multi-core) compared to the [`recommended`](https://browser.geekbench.com/v6/cpu/14205183) model.

---

### 7. Memory

* **RAM**: Minimum 2 GB (4 GB or more recommended)
* **Ballooning Device**: ❌ **Disable**

---

### 8. Network

Use the correct network adapter based on the macOS version:

| macOS Version       | Network Adapter    |
| ------------------- | ------------------ |
| macOS 11 – 26       | `VirtIO` (default) |
| macOS 10.11 – 10.15 | `VMware vmxnet3`   |
| macOS 10.4 – 10.10  | `Intel E1000`      |

---

### 9. Finalize

Don’t forget to add an **additional CD/DVD drive** for the macOS installer or macOS Recovery iso.

---

## ⚠️ Important Notes

### ISO Image Format

This is a **true DVD ISO image**.
Do **NOT** modify the VM config to change `media=cdrom` to `media=disk`.

---

## 🛠️ Troubleshooting

Having issues? Check the following:

* Secure Boot is **disabled** (`Pre-Enroll Keys` is unticked)
* ISO is mounted as a **CD/DVD**, not a disk
* You’re using a supported CPU model

---

## 🤝 Support & Contributions

Need help or want to contribute?
Visit the project on GitHub:
🔗 [**LongQT-OpenCore-ISO**](https://github.com/LongQT-sea/OpenCore-ISO)
