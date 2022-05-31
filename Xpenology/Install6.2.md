### Xpenology ds3615_6.2 on Proxmox
> source: https://xpenology.com/forum/topic/12952-dsm-62-loader/
```
wget -q -nv --no-cookies "https://github.com/vlombardino/Proxmox/blob/master/Xpenology/files/ds3615_6.2_synoboot.tar.gz?raw=true" -O /tmp/ds3615_6.2_synoboot.tar.gz
tar -zxf /tmp/ds3615_6.2_synoboot.tar.gz -C /var/lib/vz/template/iso/
rm -rf /tmp/ds3615_6.2_synoboot.tar.gz
```
Change 100.conf to correct conf number. 
```
sed -i '1s|^|args: -device ich9-usb-ehci1,id=usb-ctl-synoboot,addr=0x18 -drive id=usb-drv-synoboot,file=/var/lib/vz/template/iso/ds3615_6.2_synoboot.img,if=none,format=raw -device usb-storage,id=usb-stor-synoboot,bootindex=1,removable=off,drive=usb-drv-synoboot -device ahci,id=ahci1,multifunction=on,bus=pci.0,addr=0x8\n|' /etc/pve/qemu-server/100.conf
```

### Xpenology ds3617_6.2 on Proxmox
> source: https://xpenology.com/forum/topic/12952-dsm-62-loader/
```
wget -q -nv --no-cookies "https://github.com/vlombardino/Proxmox/blob/master/Xpenology/files/ds3617_6.2_synoboot.tar.gz?raw=true" -O /tmp/ds3617_6.2_synoboot.tar.gz
tar -zxf /tmp/ds3617_6.2_synoboot.tar.gz -C /var/lib/vz/template/iso/
rm -rf /tmp/ds3617_6.2_synoboot.tar.gz
```
Change 100.conf to correct conf number. 
```
sed -i '1s|^|args: -device ich9-usb-ehci1,id=usb-ctl-synoboot,addr=0x18 -drive id=usb-drv-synoboot,file=/var/lib/vz/template/iso/ds3617_6.2_synoboot.img,if=none,format=raw -device usb-storage,id=usb-stor-synoboot,bootindex=1,removable=off,drive=usb-drv-synoboot -device ahci,id=ahci1,multifunction=on,bus=pci.0,addr=0x8\n|' /etc/pve/qemu-server/100.conf
```

### Install FixSynoboot
> source: https://xpenology.com/forum/topic/28183-running-623-on-esxi-synoboot-is-broken-fix-available/
```
ls /dev/synoboot*
/dev/synoboot  /dev/synoboot1  /dev/synoboot2
```
If output is different from above then install FixSynoboot.sh
> example: ```ls: cannot access /dev/synoboot*: No such file or directory```

Download FixSynoboot.sh script and make it executable
```
sudo wget -q -nv --no-cookies "https://raw.githubusercontent.com/vlombardino/Proxmox/master/Xpenology/files/FixSynoboot.sh" -O /usr/local/etc/rc.d/FixSynoboot.sh
sudo chmod 0755 /usr/local/etc/rc.d/FixSynoboot.sh
```

### Enable SHR
> source: https://xpenology.club/enable-shr-in-dsm-6/
```
sudo vi /etc.defaults/synoinfo.conf
```
Insert the following above *supportphoto="yes"*
```
####################ADD TEXT####################
support_syno_hybrid_raid="yes"
################################################
```
Place a # in front *supportraidgroup="yes"*
```
####################MOD TEXT####################
#supportraidgroup="yes"
################################################
```
