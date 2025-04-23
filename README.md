# dotpy
Dotpy

```bash
nohup wget https://releases.ubuntu.com/24.04.2/ubuntu-24.04.2-desktop-amd64.iso -O /var/lib/vz/template/iso/ubuntu-24.04.2-desktop-amd64.iso & disown
```

```bash
qm create 1000 --memory 2048 --core 2 --name ubuntu-cloud --net0 virtio,bridge=vmbr0

qm disk import 1000 /var/lib/vz/template/iso/noble-server-cloudimg-amd64.img local-lvm

qm set 1000 --scsihw virtio-scsi-pci --scsi0 local-lvm:vm-1000-disk-0

qm set 1000 --ide2 local-lvm:cloudinit

qm set 1000 --boot c --bootdisk scsi0

qm set 1000 --serial0 socket --vga serial0

# set cloudinit username and password before proceeding with the next steps

qm template 1000

qm clone 1000 2001 --name ubuntu --full

qm resize 2001 scsi0 +38G
```
```bash
sudo cat /home/cloud/.ssh/id_rsa.pub | sudo tee -a /root/.ssh/authorized_keys
```

### User data:
```

```
