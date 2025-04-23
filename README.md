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
```bash
#cloud-config username: cloud password: cloud
users:
  - name: cloud
    passwd: "$6$somesalt$KLY2ZQYLjdx4lBtCH.VTkiEF4xqrFa3IkhmIiNiwxjEi1N/TWcKSO8LiXTFSBlIfKjJkGtg8mNw3ZB3DYjRvj."
    lock_passwd: false
    sudo: ALL=(ALL) NOPASSWD:ALL  # Optional: Grants passwordless sudo
    shell: /bin/bash  # Sets bash as the default shell

# Ensure password doesn't expire
chpasswd:
  expire: false

# Enable password authentication for SSH
ssh_pwauth: true

# Optional: Update SSH configuration to allow password auth
write_files:
  - path: /etc/ssh/sshd_config.d/60-cloud-init.conf
    content: |
      PasswordAuthentication yes
      ChallengeResponseAuthentication no
      UsePAM yes
```
