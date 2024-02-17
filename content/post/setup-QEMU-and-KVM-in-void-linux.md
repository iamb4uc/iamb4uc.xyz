---
title: "Setup QEMU and KVM in Void Linux"
date: 2023-08-17T22:38:08+05:30
draft: false
description: In this you will learn how to setup QEMU/KVM and manage them using virt-manager in Void Linux
tags: [blog]
---
Here are the steps on how to set this up:

1. First, become superuser
```bash
su
```

2. Update
```bash
xbps-install -Suy
```

3. Now install all the required packages 
```bash
xbps-install dbus qemu libvirtd virt-manager bridge-utils iptables2
```

4. Now link and enable the services
```bash
ln -sf /etc/sv/dbus/ /var/service/
ln -sf /etc/sv/libvirtd/ /var/service/
ln -sf /etc/sv/virtlockd/ /var/service/
ln -sf /etc/sv/virtlogd/ /var/service/
```

5. Done, Reboot
