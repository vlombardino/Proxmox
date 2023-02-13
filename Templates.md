# [Proxmox Templates](https://pve.proxmox.com/pve-docs/pveam.1.html)
## Web GUI
local -> CT Templates -> Templates

---

## Terminal 
[>_ Shell]
```bash
pveam update
pveam available
pveam available --section system
pveam available --section turnkeylinux
pveam available --section mail
pveam download <storage> <file>
pveam list <storage>
pveam remove <template_path>
```
Example to add
```bash
pveam update
pveam available --section system
pveam download local debian-11-standard_11.6-1_amd64.tar.zst
```
Example to remove
```bash
pveam list local
pveam remove local:vztmpl/debian-11-standard_11.6-1_amd64.tar.zst
```

---

## Download commands
Download all templates
```bash
pveam available | xargs -0 | awk '{print $2;}' | while read template; do pveam download <storage> $template; done
```
Download all system templates
```bash
pveam available --section system | xargs -0 | awk '{print $2;}' | while read template; do pveam download <storage> $template; done
```
Download all turnkeylinux templates
```bash
pveam available --section turnkeylinux | xargs -0 | awk '{print $2;}' | while read template; do pveam download <storage> $template; done
```
Download all mail templates
```bash
pveam available --section mail | xargs -0 | awk '{print $2;}' | while read template; do pveam download <storage> $template; done
```
