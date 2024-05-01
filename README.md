# [Install Notes](https://www.proxmox.com/en/proxmox-ve/get-started)

### Proxmox Helper
[Proxmox Helper Scripts](https://github.com/tteck/Proxmox)

---

### Folder Locations
Containers Conf ```/etc/pve/lxc/```

VM Conf ```/etc/pve/qemu-server/```

VZDump Backup ```/var/lib/vz/dump/```

ISO Folder ```/var/lib/vz/template/iso/```

Container Templates ```/var/lib/vz/template/cache/```

---

### Kill VMID
Find kvm process ID
```bash
ps aux | grep [VMID]
```
Kill kvm with the process ID
```bash
kill -9 [PID]
```

---

### Networking
Show IP Address
```bash
ip -br -c a
```

Configuration For Mode 4 Bonding
```bash
cp /etc/network/interfaces /etc/network/interfaces.bak
```
```bash
nano /etc/network/interfaces
```
```bash
#bonding
auto lo
iface lo inet loopback
iface enp35s0 inet manual
iface enp36s0 inet manual

auto bond0
iface bond0 inet manual
	bond-slaves enp35s0 enp36s0
	bond-miimon 100
#Bond Mode 4 – 802.3ad
	bond-mode 4

auto vmbr0
iface vmbr0 inet static
	address 192.168.1.11/24
 	gateway 192.168.1.1
	bridge-ports bond0
	bridge-stp off
	bridge-fd 0
```
Change host file
```bash
nano /etc/hosts
```

---

### Rename Node
Modify Host

```systemctl stop pve*```

```nano /etc/hostname``` 
> pve

```nano /etc/hosts```
> 192.168.1.XX pve.local pve

```reboot```

Move CT & VM Files
```bash
mv /etc/pve/nodes/<old_host_name>/lxc/* /etc/pve/nodes/<new_host_name>/lxc
mv /etc/pve/nodes/<old_host_name>/qemu-server/* /etc/pve/nodes/<new_host_name>/qemu-server
rm -r /etc/pve/nodes/<old_host_name>
```

Separate A Node Without Reinstalling
```bash
pvecm status
systemctl stop pve-cluster
systemctl stop corosync
pmxcfs -l
rm /etc/pve/corosync.conf
killall pmxcfs
systemctl start pve-cluster
```

---

### Disk notes
> disk/partition '/dev/sda' has a holder (500)
```
sgdisk --zap-all /dev/sda
readlink /sys/block/sda
echo 1 > /sys/block/sda/device/delete
echo "- - -" > /sys/class/scsi_host/host8/scan
```

---

## [Proxmox VE No-Subscription Repository](https://pve.proxmox.com/wiki/Package_Repositories)

### [Bullseye](https://pve.proxmox.com/wiki/Install_Proxmox_VE_on_Debian_11_Bullseye)
```bash
vim /etc/apt/sources.list.d/pve-enterprise.list

####################ADD TEXT####################
# PVE pve-no-subscription repository provided by proxmox.com,
# NOT recommended for production use
deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription
################################################
```

### [Buster](https://pve.proxmox.com/wiki/Install_Proxmox_VE_on_Debian_Buster)
```bash
vim /etc/apt/sources.list.d/pve-enterprise.list

####################ADD TEXT####################
#deb https://enterprise.proxmox.com/debian/pve buster pve-enterprise

# PVE pve-no-subscription repository provided by proxmox.com,
# NOT recommended for production use
deb http://download.proxmox.com/debian/pve buster pve-no-subscription
################################################

wget http://download.proxmox.com/debian/proxmox-ve-release-6.x.gpg -O /etc/apt/trusted.gpg.d/proxmox-ve-release-6.x.gpg
```

### [Stretch](https://pve.proxmox.com/wiki/Install_Proxmox_VE_on_Debian_Stretch)
```bash
vim /etc/apt/sources.list.d/pve-enterprise.list

####################ADD TEXT####################
#deb https://enterprise.proxmox.com/debian/pve stretch pve-enterprise

# PVE pve-no-subscription repository provided by proxmox.com,
# NOT recommended for production use
deb http://download.proxmox.com/debian/pve stretch pve-no-subscription
################################################
```
---