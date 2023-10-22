# GPU Passthrough (Win10)

### Operating System & Software
- Proxmox VM
- Windows 10
- Nvidia Drivers

---

### Edit Grub On Proxmox Server
```bash
nano /etc/default/grub
```
Intel
```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
```
AMD
```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on"
```

### Run After Editing Grub
```bash
update-grub
```

### Edit Modules
```bash
cat << EOF >> /etc/modules
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
EOF
```

### Add To Blacklist
```bash
cat << EOF >> /etc/modprobe.d/pve-blacklist.conf
blacklist radeon
blacklist nouveau
blacklist nvidia
EOF
```

### Disable GPU From Host
Check vendor GPU ID(s) of your vga card.
```bash
lspci -nn | grep -e NVIDIA
```
Create vender GPU IDs file.
```bash
lspci -nn | grep -e NVIDIA | sed -n -e 's/.*\[\([^]]*\)\].*/\1/p' | paste -s -d, | sed 's/.*/options vfio-pci ids=& disable_vga=1/' > /etc/modprobe.d/vfio.conf
```
Check output of file ```/etc/modprobe.d/vfio.conf```.
```bash
cat /etc/modprobe.d/vfio.conf
```

### If CPU is set to host
```bash
echo "options kvm ignore_msrs=1" > /etc/modprobe.d/kvm.conf
```

Reboot system.

### Sample VM configuration
```bash
cat << EOF >> /etc/pve/qemu-server/100.conf
agent: 1
args: -vnc 0.0.0.0:11
bios: ovmf
boot: order=scsi0;ide2;net0
cores: 4
cpu: host
efidisk0: NVME1:100/vm-100-disk-0.qcow2,efitype=4m,pre-enrolled-keys=1,size=528K
hostpci0: 0000:2b:00,pcie=1
machine: pc-q35-8.0
memory: 16384
meta: creation-qemu=8.0.2,ctime=1697897587
name: WinGPU
net0: virtio=BB:BB:0B:BB:11:B2,bridge=vmbr0,firewall=1
numa: 0
ostype: win11
scsi0: NVME1:100/vm-100-disk-1.qcow2,iothread=1,size=80G
scsihw: virtio-scsi-single
smbios1: uuid=d4febb0d-4444-4bdb-4aea-4c04e4b4df0e
sockets: 1
vga: none
tpmstate0: NVME1:100/vm-100-disk-2.raw,size=4M,version=v2.0
vmgenid: 524a58dd-7e3e-44f4-abf4-9de0f490d936
EOF
```

## Additional modifications

### Motherboard BIOS
>CSM -> Boot -> off

### Sources
https://github.com/techno-tim/youtube-videos/tree/master/gpu-passthrough \
https://www.reddit.com/r/homelab/comments/b5xpua/the_ultimate_beginners_guide_to_gpu_passthrough/
