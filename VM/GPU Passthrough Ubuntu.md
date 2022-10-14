# GPU Passthrough On Ubuntu VM

### Edit Grub On Proxmox Server
```
nano /etc/default/grub
```
Intel
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
```
AMD
```
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

## Create VM (Ubuntu). Install and configure driver for nvidia

### Install Nvidia Driver
```
ubuntu-drivers devices
sudo ubuntu-drivers autoinstall
```
or
```
ubuntu-drivers devices
sudo apt install nvidia-driver-515
sudo reboot
```
or
```
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:graphics-drivers/ppa -y
sudo apt update
ubuntu-drivers devices
sudo ubuntu-drivers autoinstall
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

### Disable Nouveau Driver
```
sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
sudo bash -c "echo options nouveau modset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
sudo update-initramfs -u
```
