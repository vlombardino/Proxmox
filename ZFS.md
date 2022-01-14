# ZFS Cheatsheet
List zfs pools
```
zpool list
#or
zpool list -v
```

Show stats 
```
zpool status
#or
zpool status -v
```

Show status
```
zpool iostat
#or
zpool iostat -v
```

Add cache to an existing pool
```
zpool add -f rpool cache sdf
```

Add log to an existing pool
```
zpool add -f rpool log sdg
```

Add cache and log to an existing pool
```
zpool add -f rpool log sdf1 cache sdf2
```

Scrub pool
```
zpool scrub rpool
```

Upgrade zfs pool
```
zpool upgrade rpool
```

Remove pool
```
zpool destroy rpool
```
