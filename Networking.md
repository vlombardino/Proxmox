## Networking file location
```
cat /etc/network/interfaces
```
Configuration for Mode 4 bonding
```
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
