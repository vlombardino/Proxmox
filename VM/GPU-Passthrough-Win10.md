## Setup Win 10 VM

### Edit Grub
```
vim /etc/default/grub
```

### Change the folowing lines:
```
GRUB_CMDLINE_LINUX_DEFAULT=""
#to
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
#or
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on"
```

### Run After Editing Grub
```
update-grub
```

### Edit Modules
```
cat << EOF >> /etc/modules
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
EOF
```

### Edit VM
```
cat << EOF >> /etc/pve/qemu-server/100.conf
agent: 1
bios: ovmf
bootdisk: virtio0
cores: 4
cpu: host,hidden=1
efidisk0: HDD:vm-210-disk-0,size=1M
hostpci0: 02:00,pcie=1,x-vga=1
machine: q35
memory: 16384
name: WinGPU
net0: virtio=BB:BB:0B:BB:11:B2,bridge=vmbr0
numa: 0
ostype: win10
scsihw: virtio-scsi-pci
smbios1: uuid=d4febb0d-4444-4bdb-4aea-4c04e4b4df0e
sockets: 1
vga: none
virtio0: HDD:vm-100-disk-0,cache=writethrough,size=80G
vmgenid: 524a58dd-7e3e-44f4-abf4-9de0f490d936
EOF
```

### Add To Blacklist
```
cat << EOF >> /etc/modprobe.d/blacklist.conf
blacklist radeon
blacklist nouveau
blacklist nvidia
EOF
```

## Additional modifications
### Change the folowing lines
```
GRUB_CMDLINE_LINUX_DEFAULT=""
#to
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on video=efifb:off"
```

### Add Vendor:Device IDs
```
lspci -nn | grep -e 'VGA.*NVIDIA' -e 'Audio.*NVIDIA'
echo "options vfio-pci ids=10de:1381,10de:0fbc disable_vga=1" > /etc/modprobe.d/vfio.conf
```

### Warnings In dmesg system log
```
echo "options kvm ignore_msrs=1 report_ignored_msrs=0" > /etc/modprobe.d/kvm.conf
```

### Motherboard BIOS
>CSM -> Boot -> off

### Sources
https://github.com/techno-tim/youtube-videos/tree/master/gpu-passthrough \
https://www.reddit.com/r/homelab/comments/b5xpua/the_ultimate_beginners_guide_to_gpu_passthrough/
