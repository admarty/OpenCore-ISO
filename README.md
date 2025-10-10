## About

A properly configured OpenCore **DVD/CD-format ISO file** for Proxmox VE to create macOS virtual machines.

Supports all Intel-based macOS versions — from **Mac OS X 10.4** to **macOS 26**.

**For AMD users:**

> Enjoy a true **vanilla macOS** experience with no kernel patches required for stable operation.

> This is likely the best way to run macOS on AMD hardware while still retaining full hypervisor access for other VMs.

---

## 📦 Download

Get the latest OpenCore ISO and macOS Recovery here: 👉 [Release page](https://github.com/LongQT-sea/OpenCore-ISO/releases)

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

* **Machine Type**: `q35` (if you must use `i440fx`, [cpu-models.conf](https://github.com/LongQT-sea/OpenCore-ISO/blob/main/cpu-models.conf) is required)
* **BIOS**: UEFI (OVMF)
* **Add EFI Disk**: ✅ Enabled
* **Pre-Enroll Keys**: ❌ Untick to disable Secure Boot
* **QEMU Guest Agent**:

  * ✅ Enable for macOS 10.14 – macOS 26
  * ❌ Leave as default for macOS 10.4 – macOS 10.13

---

### 5. Hard Disk

The **disk bus type** depends on your needs:

* **VirtIO** – Better performance
* **SATA** – Supports TRIM/discard for more efficient storage usage

| macOS Version            | Supports Bus Type       |
| ------------------------ | ----------------------- |
| macOS 10.15 – macOS 26   | `VirtIO Block` / `SATA` |
| macOS 10.4 – macOS 10.14 | `SATA`                  |

**Note:** SATA with ✅ **SSD emulation** and ✅ **Discard** enabled is recommended to enable TRIM/discard for better storage efficiency.

---

### 6. CPU

#### Cores

Choose based on your hardware: 1 / 2 / 4 / 8 / 16 / 32

#### Type (Model)

| macOS Version            | Recommended CPU Type                                                  |
| ------------------------ | --------------------------------------------------------------------- |
| macOS 10.11 – macOS 26   | `Broadwell-noTSX`, `Skylake-Client-v4`, `Skylake-Server-v4` (AVX-512) |
| macOS 10.4 – macOS 10.10 | `Penryn`                                                              |

> ⚠️ **Notes for AMD CPUs:**
>
> * Tick ✅ **Advanced**, and under **Extra CPU Flags**, turn off `pcid` and `spec-ctrl`.
> * For **macOS 13–26**, set the CPU manually via the Proxmox Shell, example:
>
>   ```
>   qm set [VMID] --args "-cpu Broadwell-noTSX,vendor=GenuineIntel"
>   qm set [VMID] --args "-cpu Skylake-Client-v4,vendor=GenuineIntel"
>   ```

> ⚠️ **Notes for Intel CPUs:**
>
> * Avoid using [`host` or `max`](https://browser.geekbench.com/v6/cpu/14313138) CPU types — they can be **~30% slower (single-core)** and **~44% slower (multi-core)** compared to the [`recommended`](https://browser.geekbench.com/v6/cpu/14205183) types.

---

### 7. Memory

* **RAM**: Minimum 2 GB (4 GB or more recommended)
* **Ballooning Device**: ❌ Disable

---

### 8. Network

Choose the correct adapter based on macOS version:

| macOS Version       | Network Adapter    |
| ------------------- | ------------------ |
| macOS 11 – 26       | `VirtIO` (default) |
| macOS 10.11 – 10.15 | `VMware vmxnet3`   |
| macOS 10.4 – 10.10  | `Intel E1000`      |

---

### 9. Finalize

Add an **additional CD/DVD drive** for the macOS installer or Recovery ISO.

---

## ⚠️ Important Notes

### ISO Image Format

This is a **true DVD ISO image**.
Do **NOT** modify the VM config to change `media=cdrom` to `media=disk`.

---

## 🛠️ Troubleshooting

If you encounter issues, check:

* Secure Boot is **disabled** (`Pre-Enroll Keys` unticked)
* The ISO is mounted as a **CD/DVD**, not a disk
* You’re using a **supported CPU model**

---

## 🤝 Support & Contributions

Need help or want to contribute?
Visit the project on GitHub:
🔗 [**LongQT-OpenCore-ISO**](https://github.com/LongQT-sea/OpenCore-ISO)
