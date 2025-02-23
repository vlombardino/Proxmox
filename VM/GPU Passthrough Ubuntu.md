# GPU Passthrough (Ubuntu)

### Operating System & Software
- Proxmox VM
- Ubuntu 22.04
- Nvidia Drivers

---

### Edit grub on proxmox server
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
Auto detect & modify bash command
```bash
if lscpu | grep -qi 'Intel'; then sed -i 's/^GRUB_CMDLINE_LINUX_DEFAULT=.*/GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"/' /etc/default/grub; elif lscpu | grep -qi 'AMD'; then sed -i 's/^GRUB_CMDLINE_LINUX_DEFAULT=.*/GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on"/' /etc/default/grub; else echo "Unknown CPU vendor"; fi
```

### Run after editing grub
```bash
update-grub
```

### Edit modules
```bash
cat << EOF >> /etc/modules
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd #not needed if on kernel 6.2 or newer
EOF
```
### Run After modifying modules
```bash
update-initramfs -u -k all
```

### Add to blacklist
```bash
cat << EOF >> /etc/modprobe.d/pve-blacklist.conf
blacklist nouveau
blacklist nvidia
blacklist nvidiafb
blacklist snd_hda_codec_hdmi
blacklist snd_hda_intel
blacklist snd_hda_codec
blacklist snd_hda_core
blacklist radeon
blacklist amdgpu
EOF
```

### Disable GPU from host
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
Reboot system.

---

## Create VM (Ubuntu). Install and configure driver for nvidia

### Install Nvidia driver
```bash
ubuntu-drivers devices
sudo ubuntu-drivers autoinstall
```
or
```bash
ubuntu-drivers devices
sudo apt install nvidia-driver-515
sudo reboot
```
or
```bash
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:graphics-drivers/ppa -y
sudo apt update
ubuntu-drivers devices
sudo ubuntu-drivers autoinstall
```

### Test Nvidia driver
```bash
nvidia-smi
```

### Additional software
```bash
sudo apt install qemu-guest-agent
```

## Troubleshooting

### Disable nouveau driver
```bash
sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
sudo bash -c "echo options nouveau modset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
sudo update-initramfs -u
```
### Edit proxmox config file
```bash
vim /etc/pve/qemu-server/100.conf
cpu: host,hidden=1
```
