<!--
@author: harold.duan
@date: 18-10-01
@memo: Notes logging
-->

# Gogs Notes

## Gogs Install

## Gogs Deployment

# Firewall

## iptables

## firewall

``` command
sudo firewall-cmd --zone=public --add-port=3000/tcp --permanent
sudo systemctl firewalld start/stop/restart/status == sudo service firewalld start/stop/restart/status
sudo firewall-cmd --reload
sudo systemctl unmask firewalld
sudo firewall-cmd  --list-all
sudo firewall-cmd --list-ports
```