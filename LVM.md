### Add disk to pve pool
Remove all partitions
```
sgdisk --zap-all /dev/sda
```
Create new partion
```
sgdisk -N 1 /dev/sda
```
Create physical volume
```
pvcreate /dev/sda1
```
Extend the pve volume group
```
vgextend pve /dev/sda1
```
Show available space
```
vgs
```
Extend the logical volume pve data 
```
lvextend /dev/pve/data -l +100%FREE
```

### Remove disk from pve pool
```
lvs -a -o +devices
pvdisplay
vgchange -an [VG Name]
vgremove [VG Name]
pvremove [PV Name]
```
