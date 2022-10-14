# GPU Passthrough On Ubuntu VM

### Edit Grub On Proxmox Server
```
vim /etc/default/grub
```

### Change the Folowing Lines
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
#or
pve-efiboot-tool refresh
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

### Add To Blacklist
```
cat << EOF >> /etc/modprobe.d/blacklist.conf
blacklist radeon
blacklist nouveau
blacklist nvidia
EOF
```

### Disable GPU From Host
Find the device and vendor id of your vga card ```[***:***]```.
```
lspci -nn | grep -e 'VGA.*NVIDIA' -e 'Audio.*NVIDIA'
```

Insert Vender GPU IDs & Update
```
echo "options vfio-pci ids=****:****,****:*** disable_vga=1"> /etc/modprobe.d/vfio.conf
```
```
update-initramfs -u
```

## Create VM (Ubuntu 20.04). Install and configure driver for nvidia

### Disable Nouveau Driver
```
sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
sudo bash -c "echo options nouveau modset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
sudo update-initramfs -u
```

### Install Nvidia Driver
```
sudo apt update
sudo apt install build-essential libglvnd-dev pkg-config
wget https://us.download.nvidia.com/XFree86/Linux-x86_64/455.23.04/NVIDIA-Linux-x86_64-455.23.04.run
chmod +x NVIDIA-Linux-x86_64-455.23.04.run
sudo ./NVIDIA-Linux-x86_64-455.23.04.run
sudo reboot
#or
ubuntu-drivers devices
sudo apt install nvidia-driver-440
sudo reboot
```

### Test Nvidia Driver
```
nvidia-smi
```

### Edit Proxmox Config File
```
vim /etc/pve/qemu-server/100.conf
cpu: host,hidden=1
```

### Additional Software
```
sudo apt install qemu-guest-agent
```
