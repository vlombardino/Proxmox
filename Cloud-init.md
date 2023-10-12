Create cloud VM
```
qm create 1000 --memory 2048 --core 2 --name ubuntu-cloud --net0 virtio,bridge=vmbr0
```

Download [Ubuntu cloud image](https://cloud-images.ubuntu.com)
```
cd var/lib/vz/template/iso/
wget https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64-disk-kvm.img
```

Import cloud image to VM ubuntu-cloud
> HDD0 is storage name
```
qm importdisk 1000 jammy-server-cloudimg-amd64-disk-kvm.img HDD0
```

Configure drive for VM
> Use the commands below or modify through Hardware & Options
```
qm set 1000 --scsihw virtio-scsi-pci --scsi0 HDD0:vm-1000-disk-0
qm set 1000 --ide2 HDD0:cloudinit
qm set 1000 --boot c --bootdisk scsi0
```
