## Proxmox Helper
[Proxmox Helper Scripts](https://github.com/tteck/Proxmox)

---

## Folder Locations
Containers Conf ```/etc/pve/lxc/```

VM Conf ```/etc/pve/qemu-server/```

VZDump Backup ```/var/lib/vz/dump/```

ISO Folder ```/var/lib/vz/template/iso/```

Container Templates ```/var/lib/vz/template/cache/```

---

## Networking
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

## Dark Theme
```
wget https://raw.githubusercontent.com/Weilbyte/PVEDiscordDark/master/PVEDiscordDark.py

python3 PVEDiscordDark.py
```

## Sources
https://github.com/Weilbyte/PVEDiscordDark

---

## Rename Node
### Modify Host
```systemctl stop pve*```

```nano /etc/hostname``` 
> pve

```nano /etc/hosts```
> 192.168.1.XX pve.local pve

```reboot```

### Move CT & VM Files
```bash
mv /etc/pve/nodes/<old_host_name>/lxc/* /etc/pve/nodes/<new_host_name>/lxc
mv /etc/pve/nodes/<old_host_name>/qemu-server/* /etc/pve/nodes/<new_host_name>/qemu-server
rm -r /etc/pve/nodes/<old_host_name>
```

### Separate A Node Without Reinstalling
```bash
pvecm status
systemctl stop pve-cluster
systemctl stop corosync
pmxcfs -l
rm /etc/pve/corosync.conf
killall pmxcfs
systemctl start pve-cluster
```
