## Debian | Ubuntu
Check ssh status
```bash
systemctl status ssh
```
Enable and/or start ssh
```bash
systemctl enable ssh
systemctl start ssh
```
Edit sshd_config file
```bash
nano /etc/ssh/sshd_config
#or
vim.tiny /etc/ssh/sshd_config

####################ADD TEXT####################
PermitRootLogin yes
################################################

service ssh restart
```

## Fedora
```bash
dnf install openssh-server

nano /etc/ssh/sshd_config
#or
vi /etc/ssh/sshd_config

####################ADD TEXT####################
PermitRootLogin yes
################################################

systemctl restart sshd
```

## Rocky
```bash
dnf install openssh-server
systemctl start sshd
```
