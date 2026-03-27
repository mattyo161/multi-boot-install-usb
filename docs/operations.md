# Operations and Maintenance

This document covers day-2 operations after initial USB build.

## Update Installer ISOs

1. Download new ISO versions.
2. Mount Ventoy data partition.
3. Remove old ISO files.
4. Copy in new ISO files.
5. Reboot and test menu entries.

No Ventoy reinstall is needed for routine ISO refreshes.

## Update Ventoy

When upgrading Ventoy itself:

1. Read Ventoy release notes.
2. Run Ventoy update mode on the same USB.
3. Re-test ISO boots and utilities OS boot.

Always keep backups of important files before bootloader updates.

## Update Utilities OS

From utilities Mint:

```bash
sudo apt update
sudo apt full-upgrade -y
sudo apt autoremove -y
```

Reboot after kernel updates and confirm tools still work.

## Backup Recommendations

- Keep a local copy of all ISO files.
- Export package list from utilities OS:

```bash
dpkg --get-selections > ~/util-mint-packages.txt
```

- Save critical scripts/config in version control.

## Troubleshooting

### USB does not appear in boot list

- Re-check firmware boot mode (UEFI/Legacy)
- Try another USB port (prefer direct motherboard ports)
- Recreate Ventoy on USB if partition table is damaged

### Ventoy menu appears but ISO fails to boot

- Verify ISO checksums
- Re-copy ISO (possible corruption during transfer)
- Test with latest Ventoy release

### Utilities OS missing from boot entries

- Boot from live media and inspect EFI entries:

```bash
sudo efibootmgr -v
```

- Reinstall GRUB to USB target:

```bash
sudo grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=UTIL_MINT --removable
sudo update-grub
```

### Internal disk was modified accidentally

- Stop and document what changed
- Boot rescue media
- Inspect partition table with `gdisk -l` or `parted -l`
- Restore from backups before further writes

## Future Enhancements

- Add Secure Boot compatibility notes
- Add checksum automation script for ISO validation
- Add hardware compatibility matrix
- Add optional persistent storage strategy for live sessions
