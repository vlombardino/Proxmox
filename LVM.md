### Add disk to pve pool
Remove all partitions
```bash
sgdisk --zap-all /dev/sda
```
Create new partion
```bash
sgdisk -N 1 /dev/sda
```
Create physical volume
```bash
pvcreate /dev/sda1
```
Extend the pve volume group
```bash
vgextend pve /dev/sda1
```
Show available space
```bash
vgs
```
Extend the logical volume pve data 
```bash
lvextend /dev/pve/data -l +100%FREE
```

### Remove disk from pve pool
```bash
lvs -a -o +devices
pvdisplay
vgchange -an [VG Name]
vgremove [VG Name]
pvremove [PV Name]
```
