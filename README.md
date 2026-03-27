# Multi-Boot Install USB Drive

Build a single USB drive that can:

- Boot multiple installer ISOs from a GRUB-based menu path (via Ventoy)
- Install Linux Mint and Ubuntu variants
- Boot into a dedicated utilities OS for rescue, diagnostics, and disk work

This repository documents a repeatable pattern you can adapt for your own hardware.

## Goals

- Multi-boot installer USB for:
  - Latest Linux Mint
  - Latest Ubuntu Desktop
  - Ubuntu Server (minimal-style install path)
- Dedicated utilities partition with a minimal Mint install
- Practical post-install toolset for partitioning, networking, and volume inspection

## Recommended Approach

Use **Ventoy** for ISO boot management and reserve free space on the same USB for a full Linux utilities installation.

Why this design:

- Ventoy is easy to maintain: replace ISO files without rebuilding the whole drive
- ISO boot menu is reliable across many distros
- Utilities environment is a full Linux install (better than fragile live persistence)

## Repository Contents

- `docs/build-guide.md`: end-to-end setup instructions
- `docs/operations.md`: maintenance, updates, and recovery notes

## Quick Start

1. Read `docs/build-guide.md` completely.
2. Identify the correct USB target device.
3. Install Ventoy with reserved space.
4. Copy ISOs to Ventoy partition.
5. Install minimal Mint into reserved partition.
6. Validate boot paths on UEFI hardware.

## Safety

The commands in this project can erase disks if used on the wrong device.

- Double-check `/dev/sdX` every time.
- Disconnect non-target external drives when possible.
- Keep backups before disk operations.

## Suggested USB Size

- **Minimum:** 64 GB
- **Preferred:** 128 GB+ (more room for ISOs + utility tooling + logs)

## Suggested Boot Modes

- UEFI-first workflow (recommended)
- Legacy BIOS support as optional fallback

## Contributing

Contributions are welcome for:

- Additional distro profiles
- Secure Boot notes
- Hardware compatibility reports
- Improved validation checklists
