## Passthrough Physical Disk To VM

### Identify Disk
> Show ```/dev/disk/by-id/``` output.
```bash
find /dev/disk/by-id/ -type l|xargs -I{} ls -l {}|grep -v -E '[0-9]$' |sort -k11|cut -d' ' -f9,10,11,12
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

### Remove All Partitions & Data
```bash
dd if=/dev/zero of=/dev/sdX bs=1M status=progress
```
or
```bash
wipefs -a /dev/sdX
```
