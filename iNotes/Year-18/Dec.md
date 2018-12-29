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

``` Printscreen
Printscreen – 截取整个桌面的屏幕截图并保存你的照片文件夹
Alt + Printscreen – 截取目前获得焦点的窗口截图并保存你的照片文件夹
Shift + Printscreen – 选定区域截取并将其保存到你的照片文件夹
Ctrl + Printscreen – 截取整个桌面的屏幕截图并将其复制到剪贴板
Ctrl + Alt + Printscreen – 截取目前获得焦点的窗口截图并将其复制到剪贴板
Ctrl + Shift + Printscreen – 选定区域截取并将其复制到剪贴板
```

\#|硬件|参数|备注
--|---|---|---
1|CPU|4核及以上|
2|内存|32G及以上|
3|硬盘|2T及以上|
4|带宽速度|客户端净单点下载速度希望能在100KB/S及以上|