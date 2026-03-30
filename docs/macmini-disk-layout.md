# macmini disk layout

## Boot into the EFI/Grub boot loader

1. Hold down the `option` key during startup to bring up Apples boot disk menu. Select the EFI Boot drive that was setup previously
2. When you get to the Grub menu type `c` to enter the console
3. Enable the pager `set pager=1`
4. Get the list of drives using `ls -l` you will want to capture the UUID of the Mac volumes as they will be different the what unix will give you with `blkid`
5. Enter the UUIDs into the table below


## /dev/disk0 (internal, physical):
|  ID |                    TYPE | NAME                  |   SIZE      |  IDENTIFIER  | UUID |
| - | ------ | ------ | ----- | --- | --- |
|   0 |   GUID_partition_scheme |                       |  *500.1 GB  |  disk0  | 70D6-1701 |
|   1 |                     EFI | EFI                   |   209.7 MB  |  disk0s1  |  |
|   2 |               Apple_HFS | Lion                  |   29.5 GB   |  disk0s2  | c8974fa640f4b164 |
|   3 |               Apple_HFS | Recovery HD           |   650.0 MB  |  disk0s3  |  |
|   4 |               Apple_HFS | Mavericks             |   29.5 GB   |  disk0s4  | 72ce301e9438c8c1 |
|   5 |              Apple_Boot | Recovery HD           |   650.0 MB  |  disk0s5  |  |
|   6 |               Apple_HFS | Yosemite              |   29.5 GB   |  disk0s6  | 644e6ee274d25c5f |
|   7 |              Apple_Boot | Recovery HD           |   650.0 MB  |  disk0s7  |  |
|   8 |               Apple_HFS | Sierra                |   29.5 GB   |  disk0s8  | 4a7255880f49427f |
|   9 |              Apple_Boot | Recovery HD           |   650.0 MB  |  disk0s9  |  |
|  10 |              Apple_APFS | UBUNTUK3S             |   52.4 GB   |  disk0s10  | c798e89f-e1e6-49a4-a16a-046949b416ba |
|  11 |              Linux Swap | SWAP                  |   21.0 GB   |  disk0s11  | 5e353f09-d5a2-4fe6-9c01-6e6dd8a9b7b9 |
|  12 |        Linux Filesystem | MINTOS                |   83.9 GB   |  disk0s12  | 71e35088-d620-49fa-9a6d-00592765ccdf |
|  13 |    Microsoft Basic Data | DATA                  |   83.9 GB   |  disk0s13  | 9AE9-739C |
|  14 |              Apple_APFS | High Siera.           |   31.5 GB   |  disk0s14  | b5afb987-743d-4ca9-bf82-739b84e85ee4 |
|  15 |               Apple_HFS | ElCapitan             |   21.0 GB   |  disk0s15  |  d9222bb177856e09 |

## Details on working with MacOS Volumes

- https://eclecticlight.co/2021/11/25/copying-a-mac-volume-to-another-file-system-and-back/

## Update `/etc/grub.d`

With the layout above we can now update our `/etc/grub.d/08_custom` file. This allows us to add a number of custom entries in the order that we want ahead of anything else loaded in grub. More details on grub may follow in the future but for now you can run the following commands on your Linux partition where grub is installed:

```shell
touch /etc/grub.d/08_custom
chmod +x /etc/grub.d/08_custom
# edit the file with vi, nano, ...

# After you update the file you will need to update grub
sudo update-grub
```

### Contents of `/etc/grub.d/08_custom`

```grub
#!/bin/sh
exec tail -n +3 $0
# This file provides an easy way to add custom menu entries.  Simply type the
# menu entries you want to add after this comment.  Be careful not to change
# the 'exec tail' line above.

menuentry "Linux Mint 22.3 Zena" {
    insmod part_gpt
    insmod ext2
    search --no-floppy --fs-uuid --set=root 71e35088-d620-49fa-9a6d-00592765ccdf
    linux /boot/vmlinuz root=UUID=71e35088-d620-49fa-9a6d-00592765ccdf ro quiet splash
    initrd /boot/initrd.img
}
menuentry "Ubuntu 24.04.4 LTS (24.04)" {
    insmod part_gpt
    insmod ext2
    search --no-floppy --fs-uuid --set=root 51ec7527-3f0f-4441-b789-8ced336fed80
    linux /boot/vmlinuz root=UUID=51ec7527-3f0f-4441-b789-8ced336fed80 ro quiet splash
    initrd /boot/initrd.img
}
menuentry "MacOS High Sierra (10.13)" {
    insmod part_gpt
    insmod hfsplus
    search --no-floppy --fs-uuid --set=root d9222bbb5afb987-743d-4ca9-bf82-739b84e85ee4177856e09
    chainloader /System/Library/CoreServices/boot.efi
}
menuentry "MacOS Sierra (10.12)" {
    insmod part_gpt
    insmod hfsplus
    search --no-floppy --fs-uuid --set=root 4a7255880f49427f
    chainloader /System/Library/CoreServices/boot.efi
}
menuentry "MacOS El Capitan (10.11)" {
    insmod part_gpt
    insmod hfsplus
    search --no-floppy --fs-uuid --set=root d9222bb177856e09
    chainloader /System/Library/CoreServices/boot.efi
}
menuentry "MacOS Yosemite (10.10)" {
    insmod part_gpt
    insmod hfsplus
    search --no-floppy --fs-uuid --set=root 644e6ee274d25c5f
    chainloader /System/Library/CoreServices/boot.efi
}
menuentry "MacOS Mavericks (10.9)" {
    insmod part_gpt
    insmod hfsplus
    search --no-floppy --fs-uuid --set=root 72c33e498dc80fa4
    chainloader /System/Library/CoreServices/boot.efi
}
menuentry "MacOS Mountain Lion (10.8)" {
    insmod part_gpt
    insmod hfsplus
    search --no-floppy --fs-uuid --set=root c8974fa640f4b164
    chainloader /System/Library/CoreServices/boot.efi
}
```
