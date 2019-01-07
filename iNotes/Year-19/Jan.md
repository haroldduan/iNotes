<!--
@author: harold.duan
@date: 19-01-01
@memo: Notes logging
-->

# VirtualBox auto start service

``` Command
# register vm
vboxmanage registervm /home/admin/VirtualBox\ VMs/b1srv-win2016/b1srv-win2016.vbox

# search vms list
vboxmanage list vms

# set vm env
sudo vim /etc/default/virtualbox
    VBOXAUTOSTART_DB=/etc/vbox
    VBOXAUTOSTART_CONFIG=/etc/vbox/vboxauto.conf

# set start user
sudo vim /etc/vbox/vboxauto.conf
    default_policy = deny
    admin= {
    allow = true
    }

# alter mod folder
sudo chmod 1775 /etc/vbox

# set autostart property
vboxmanage setproperty autostartdbpath /etc/vbox

# seach vms properties
vboxmanage list systemproperties

# set vm auto start
vboxmanage modifyvm b1srv-win2016 --autostart-enabled on --autostop-type acpishutdown
```