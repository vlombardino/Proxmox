## OpenVPN Client
> privileged container

### Edit CT Conf
```
nano /etc/pve/lxc/xxx.conf

lxc.mount.entry: /dev/net/tun dev/net/tun none bind,create=file
```

### Check TUN/TAP
```
cd /dev/net/
ls
```

### Install OpenVPN
```
wget https://git.io/vpn -O openvpn-install.sh
bash openvpn-install.sh
```

## Sources
https://pve.proxmox.com/wiki/OpenVPN_in_LXC
