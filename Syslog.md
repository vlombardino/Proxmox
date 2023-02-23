# Syslog Notes

## aliases error
```bash
Apr 02 20:26:54 pve postfix/local[6472]: warning: hash:/etc/aliases is unavailable. open database /etc/aliases.db: No such file or directory
Apr 02 20:26:54 pve postfix/local[6472]: warning: hash:/etc/aliases: lookup of 'root' failed
Apr 02 20:26:54 pve postfix/local[6473]: error: open database /etc/aliases.db: No such file or directory
```

Fix by rebuilding `/etc/aliases.db`. Run the following command:
```
newaliases
```

Check if error persist:
```
journalctl -f
```