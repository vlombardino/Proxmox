Edit VM configuration file
> Hardware -> Display -> Default
```
nano /etc/pve/local/qemu-server/22.conf
```

Add argument to VM configuration file.
```
args: -vnc 0.0.0.0:22
```

VNC Settings:
```
Server: 192.168.1.91:5922
```
> * No other settings need to be added
