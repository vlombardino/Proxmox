## Debian | Ubuntu
Check ssh status
```
systemctl status ssh
```
Enable and/or start ssh
```
systemctl enable ssh
systemctl start ssh
```
Edit sshd_config file
```
nano /etc/ssh/sshd_config
#or
vim.tiny /etc/ssh/sshd_config

####################ADD TEXT####################
PermitRootLogin yes
################################################

service ssh restart
```

## Fedora
```
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
```
dnf install openssh-server
systemctl start sshd
```
