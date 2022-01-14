## Proxmox VE No-Subscription Repository

### bullseye
```
vim /etc/apt/sources.list.d/pve-enterprise.list

####################ADD TEXT####################
# PVE pve-no-subscription repository provided by proxmox.com,
# NOT recommended for production use
deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription
################################################
```

### buster
```
vim /etc/apt/sources.list.d/pve-enterprise.list

####################ADD TEXT####################
#deb https://enterprise.proxmox.com/debian/pve buster pve-enterprise

# PVE pve-no-subscription repository provided by proxmox.com,
# NOT recommended for production use
deb http://download.proxmox.com/debian/pve buster pve-no-subscription
################################################

wget http://download.proxmox.com/debian/proxmox-ve-release-6.x.gpg -O /etc/apt/trusted.gpg.d/proxmox-ve-release-6.x.gpg
```

### stretch
```
vim /etc/apt/sources.list.d/pve-enterprise.list

####################ADD TEXT####################
#deb https://enterprise.proxmox.com/debian/pve stretch pve-enterprise

# PVE pve-no-subscription repository provided by proxmox.com,
# NOT recommended for production use
deb http://download.proxmox.com/debian/pve stretch pve-no-subscription
################################################
