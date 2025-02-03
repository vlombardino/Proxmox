## Passthrough Physical Disk To VM

### Identify Disk
> Show ```/dev/disk/by-id/``` output.
```bash
find /dev/disk/by-id/
```
> Cleaner output.
```bash
s=$(printf '%*s' "${COLUMNS:-$(tput cols)}" '' | tr ' ' '-'); echo -e "$s\nDisk | Size | Device\n$s"; lsblk -ndo NAME,SIZE | grep -v '^loop' | while read -r d z; do echo "$d | $z | $(find /dev/disk/by-id -type l ! -name '*nvme-eui*' ! -name '*wwn*' -lname "*$d" -printf %p -quit || echo 'N/A')"; done
```

### Add Disk To VM
> Change *-scsi1* or *-sata1* to an available Bus/Device and replace ```ata-Samsung_...``` with the output from the above command. 
```bash
qm set 100 -scsi1 /dev/disk/by-id/ata-Samsung_SSD_840_EVO_120GB_S1D1NSBF111111L
```
> Output ```update VM 100: -scsi1 /dev/disk/by-id/ata-Samsung_SSD_840_EVO_120GB_S1D1NSBF111111L```

or
```bash
qm set 100 -sata1 /dev/disk/by-id/ata-Samsung_SSD_840_EVO_120GB_S1D1NSBF111111L
```
> Output ```update VM 100: -sata1 /dev/disk/by-id/ata-Samsung_SSD_840_EVO_120GB_S1D1NSBF111111L```

## Additional Notes

### Remove all partitions & data
```bash
dd if=/dev/zero of=/dev/sdX bs=512 count=1 status=progress
```
or
```bash
wipefs -a /dev/sdX
```
