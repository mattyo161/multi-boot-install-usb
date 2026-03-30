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

```
/dev/sda10: LABEL="UBUNTUK3S" UUID="51ec7527-3f0f-4441-b789-8ced336fed80" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="Macintosh HD" PARTUUID="c798e89f-e1e6-49a4-a16a-046949b416ba"
/dev/sda11: UUID="5e353f09-d5a2-4fe6-9c01-6e6dd8a9b7b9" TYPE="swap" PARTLABEL="SWAP" PARTUUID="ecdcf219-828d-40ca-9188-759da4c766d8"
/dev/sda12: LABEL="MINTOS" UUID="71e35088-d620-49fa-9a6d-00592765ccdf" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="MINTOS" PARTUUID="0a4b5961-9965-4e9c-9b40-42ef77f96f6c"
/dev/sda13: LABEL_FATBOOT="DATA" LABEL="DATA" UUID="9AE9-739C" BLOCK_SIZE="512" TYPE="vfat" PARTLABEL="DATA" PARTUUID="d96cd2a8-95e1-4b14-acf1-c834f3c50ab6"
/dev/sda14: UUID="b5afb987-743d-4ca9-bf82-739b84e85ee4" BLOCK_SIZE="4096" TYPE="apfs" PARTLABEL="HighSierra" PARTUUID="00bb30c2-cea5-45c7-ad7d-d5f6f145ace1"
/dev/sda15: UUID="f5cc041d-cfc0-393e-80ac-56025d4dc1bf" BLOCK_SIZE="4096" LABEL="ElCapitan" TYPE="hfsplus" PARTLABEL="Sierra" PARTUUID="4c00e2bc-48af-41db-99b2-b9f61eb0c3b3"
/dev/sda1: LABEL_FATBOOT="EFI" LABEL="EFI" UUID="70D6-1701" BLOCK_SIZE="512" TYPE="vfat" PARTLABEL="EFI System Partition" PARTUUID="a61654b4-158b-4ab7-b169-0cbfff71f2f4"
/dev/sda2: UUID="8535fe97-fdbe-3d5f-8348-ca8e6cb6ac1d" BLOCK_SIZE="4096" LABEL="Lion" TYPE="hfsplus" PARTLABEL="Lion" PARTUUID="64157ee4-312f-4ae4-924b-a89718c9faac"
/dev/sda3: UUID="737b231f-7213-3fd4-9873-44fb273fd06e" BLOCK_SIZE="4096" LABEL="Recovery HD" TYPE="hfsplus" PARTLABEL="Recovery HD" PARTUUID="67c23b46-f0ac-4523-850d-f438c37b8a23"
/dev/sda4: UUID="273f0d84-ecfe-3746-b8ff-e7422c568518" BLOCK_SIZE="4096" LABEL="Mavericks" TYPE="hfsplus" PARTLABEL="Mavericks" PARTUUID="d62bb931-de3b-463a-b3a0-ec23728dabeb"
/dev/sda5: UUID="b3d95b49-2d75-3ee7-acb6-a8a4e70205bd" BLOCK_SIZE="4096" LABEL="Recovery HD" TYPE="hfsplus" PARTLABEL="Recovery HD" PARTUUID="ae42f7f7-999c-40c1-adfa-d534fac70616"
/dev/sda6: UUID="ff5e0956-54e0-333a-b107-47fe68bdffe4" BLOCK_SIZE="4096" LABEL="Yosemite" TYPE="hfsplus" PARTLABEL="Yosemite" PARTUUID="6d0f114c-0657-49f9-be09-bb9e652755ef"
/dev/sda7: UUID="432c4085-d838-3692-9666-b42c8852d1d0" BLOCK_SIZE="4096" LABEL="Recovery HD" TYPE="hfsplus" PARTLABEL="Recovery HD" PARTUUID="1402808d-b69c-4dd7-9dec-97b86a148764"
/dev/sda8: UUID="a0a36df6-b9c7-378c-b37d-f216b7de7de9" BLOCK_SIZE="4096" LABEL="Sierra" TYPE="hfsplus" PARTLABEL="El Capitan" PARTUUID="27e22adb-91b6-4231-8f85-c4907392f61e"
/dev/sda9: UUID="c4f9598b-febc-3877-a5c7-6207c2479950" BLOCK_SIZE="4096" LABEL="Recovery HD" TYPE="hfsplus" PARTLABEL="Recovery HD" PARTUUID="8a435016-8041-4686-9d29-dfdc4a681984"
```


/dev/disk1 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +31.5 GB    disk1
                                 Physical Store disk0s14
   1:                APFS Volume HighSierra              19.0 GB    disk1s1
   2:                APFS Volume Preboot                 21.7 MB    disk1s2
   3:                APFS Volume Recovery                512.1 MB   disk1s3
   4:                APFS Volume VM                      20.5 KB    disk1s4

## Details on working with MacOS Volumes

- https://eclecticlight.co/2021/11/25/copying-a-mac-volume-to-another-file-system-and-back/