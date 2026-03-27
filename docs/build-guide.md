# Build Guide: Multi-OS USB + Utilities Partition

This guide creates one USB drive with:

- Ventoy-managed multi-ISO boot menu
- Installer ISOs for Mint and Ubuntu variants
- A dedicated ext4 partition for a minimal Mint utilities environment

---

## 1) Prerequisites

- Linux workstation with `sudo`
- USB drive (64 GB minimum, 128 GB+ recommended)
- Stable internet connection for ISO downloads
- Optional second USB for safer utilities-OS installation workflow

Install required host tools:

```bash
sudo apt update
sudo apt install -y curl wget parted gdisk lsblk
```

---

## 2) Download Installer ISOs

Download the latest releases from official sources:

- Linux Mint
- Ubuntu Desktop
- Ubuntu Server (recommended minimal path)

Store them in a staging directory (example):

```bash
mkdir -p ~/iso-staging
```

---

## 3) Download Ventoy

Official links (use these if filenames or release URLs change):

- Ventoy website: [https://www.ventoy.net/](https://www.ventoy.net/)
- Ventoy releases: [https://github.com/ventoy/Ventoy/releases](https://github.com/ventoy/Ventoy/releases)

Simple download + extract flow (replace version when a new release is published):

```bash
mkdir -p ~/tools/ventoy && cd ~/tools/ventoy
VENTOY_VERSION="1.1.10"
curl -fL -o "ventoy-${VENTOY_VERSION}-linux.tar.gz" \
  "https://github.com/ventoy/Ventoy/releases/download/v${VENTOY_VERSION}/ventoy-${VENTOY_VERSION}-linux.tar.gz"
tar -xzf "ventoy-${VENTOY_VERSION}-linux.tar.gz"
cd "ventoy-${VENTOY_VERSION}"
chmod +x Ventoy2Disk.sh
```

If the command fails due to a version mismatch, check the releases page and update `VENTOY_VERSION`.

---

## 4) Identify the USB Device

Plug in the target USB and confirm device path:

```bash
lsblk -o NAME,SIZE,MODEL,TRAN
```

Assume target is `/dev/sdX` in the steps below.

> WARNING: If `/dev/sdX` is wrong, you may destroy data on another disk.

---

## 5) Install Ventoy and Reserve Space

Extract Ventoy and run:

```bash
sudo ./Ventoy2Disk.sh -i -r 30000 /dev/sdX
```

- `-i`: install Ventoy
- `-r 30000`: reserve ~30 GB at the end of disk for utilities OS

After install, verify layout:

```bash
sudo parted /dev/sdX print
sudo parted /dev/sdX print free
```

Typical result:

- Ventoy data partition (large exFAT)
- Ventoy EFI partition
- Unallocated reserved free space (for utilities OS)

---

## 6) Copy ISOs to Ventoy Data Partition

Mount the Ventoy data partition and copy ISOs:

```bash
cp ~/iso-staging/*.iso /path/to/ventoy-data/
sync
```

Ventoy automatically detects ISOs and adds them to its boot menu.

---

## 7) Create Utilities Partition

Use the reserved free space:

```bash
sudo parted /dev/sdX print free
sudo parted /dev/sdX mkpart UTIL_MINT ext4 <start> <end>
sudo mkfs.ext4 -L UTIL_MINT /dev/sdX3
```

Replace `<start>` and `<end>` with exact boundaries from `print free`.

---

## 8) Install Minimal Mint to Utilities Partition

Boot from a Mint installer (or separate live USB) and launch installer.

Recommended manual partitioning:

- Root (`/`): `/dev/sdX3` (ext4, format enabled)
- EFI System Partition: Ventoy EFI partition (usually `/dev/sdX2`, do not format)
- Bootloader target: `/dev/sdX` (the USB disk, not internal drive)

After installation:

- Reboot from USB
- Confirm you can reach both:
  - Ventoy ISO menu path
  - Installed Mint utilities OS

---

## 9) Utilities Toolset

Once booted into utilities Mint:

```bash
sudo apt update
sudo apt install -y \
  gparted gdisk parted testdisk smartmontools nvme-cli \
  nmap net-tools iperf3 traceroute wireshark \
  cifs-utils nfs-common ntfs-3g exfatprogs btrfs-progs lvm2 mdadm \
  cryptsetup rsync gddrescue curl wget git gnome-disk-utility
```

Optional:

```bash
sudo apt install -y baobab filelight
```

---

## 10) Validation Checklist

- USB boots on UEFI hardware
- Ventoy menu lists all copied ISOs
- Mint installer ISO boots successfully
- Ubuntu Desktop ISO boots successfully
- Ubuntu Server ISO boots successfully
- Utilities Mint boots successfully
- Disk tools run (`gparted`, `parted`, `lsblk`)
- Network tools run (`nmap`, `iperf3`, `traceroute`)
- Volume inspection works (NTFS/exFAT/btrfs/LVM/RAID)

---

## 11) Notes on "Minimal Ubuntu"

Two common paths:

1. **Ubuntu Desktop installer with "minimal installation" option**
2. **Ubuntu Server install** (lean baseline suitable for minimal setups)

Use whichever better matches your deployment goals.