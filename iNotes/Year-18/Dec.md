<!--
@author: harold.duan
@date: 18-12-01
@memo: Notes logging
-->

# Centos KVM

## Virtual Bridge

## Windows's Virtio drivers

## VirtualBox

``` Command
# VirtualBox run backround
# VBoxManage startvm <uuid>|<name> [--type gui|sdl|headless]
vboxmanage startvm winsrv2016 --type headless
# VirtualBox control
# VBoxManage controlvm <uuid>|<name> pause|resume|reset|poweroff|savestate|
VBoxManage controlvm winsrv2016 poweroff
# automatic run
# /etc/rc.local
# chmod +x /etc/rc.d/rc.local
# sudo chmod +x /etc/rc.local
# fixing vbox's bug
sudo -u admin vboxmanage startvm winsrv2016 --type headless
```

``` Command
# boot log
/var/log/boot.log
```