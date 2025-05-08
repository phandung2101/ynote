List disk with ID

```bash
lsblk |awk 'NR==1{print $0" DEVICE-ID(S)"}NR>1{dev=$1;printf $0" ";system("find /dev/disk/by-id -lname \"*"dev"\" -printf \" %p\"");print "";}'|grep -v -E 'part|lvm'

```

```
qm set 592 -scsi2 /dev/disk/by-id/ata-WDC_WD20SPZX-00UA7T0_WD-WX42AC3RWDJ0

```