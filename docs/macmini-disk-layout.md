# macmini disk layout

## Boot into the EFI/Grub boot loader

Hold down the `option` key during startup to bring up Apples boot disk menu. Select the EFI Boot drive that was setup previously

When you get to the Grub menu type `c` to enter the console

Enable the pager `set pager=1`

Get the list of drives using `ls -l` you will want to capture the UUID of the Mac volumes as they will be different the what unix will give you with `blkid`



## /dev/disk0 (internal, physical):
|  ID |                    TYPE | NAME                  |   SIZE      |  IDENTIFIER  | UUID |
| - | ------ | ------ | ----- | --- | --- |
|   0 |   GUID_partition_scheme |                       |  *500.1 GB  |  disk0  |  |
|   1 |                     EFI | EFI                   |   209.7 MB  |  disk0s1  |  |
|   2 |               Apple_HFS | Lion                  |   29.5 GB   |  disk0s2  | c8974fa640f4b164 |
|   3 |               Apple_HFS | Recovery HD           |   650.0 MB  |  disk0s3  |  |
|   4 |               Apple_HFS | Mavericks             |   29.5 GB   |  disk0s4  | 72ce301e9438c8c1 |
|   5 |              Apple_Boot | Recovery HD           |   650.0 MB  |  disk0s5  |  |
|   6 |               Apple_HFS | Yosemite              |   29.5 GB   |  disk0s6  | 644e6ee274d25c5f |
|   7 |              Apple_Boot | Recovery HD           |   650.0 MB  |  disk0s7  |  |
|   8 |               Apple_HFS | Sierra                |   29.5 GB   |  disk0s8  | 4a7255880f49427f |
|   9 |              Apple_Boot | Recovery HD           |   650.0 MB  |  disk0s9  |  |
|  10 |              Apple_APFS |                       |   52.4 GB   |  disk0s10  |  |
|  11 |              Linux Swap |                       |   21.0 GB   |  disk0s11  |  |
|  12 |        Linux Filesystem |                       |   83.9 GB   |  disk0s12  |  |
|  13 |    Microsoft Basic Data | DATA                  |   83.9 GB   |  disk0s13  |  |
|  14 |              Apple_APFS | Container disk1       |   31.5 GB   |  disk0s14  |  |
|  15 |               Apple_HFS | ElCapitan             |   21.0 GB   |  disk0s15  |  d9222bb177856e09 |

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