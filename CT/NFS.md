### NFS Mount
```
vim /etc/apparmor.d/abstractions/lxc/container-base

# allow NFS
mount fstype=nfs,
mount fstype=nfs4,
mount fstype=rpc_pipefs,

service apparmor restart
```

### NFS Mount Local Machine
```
/etc/pve/lxc/<ID>.conf
mp0:/mount/point/on/host,mp=/mount/point/on/lxc
mp1:/mount/point/on/host,mp=/mount/point/on/lxc
```
