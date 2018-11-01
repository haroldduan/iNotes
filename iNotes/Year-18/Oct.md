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
sudo systemctl firewalld start\stop\restart\status == sudo service firewalld start\stop\restart\status
sudo firewall-cmd --reload
sudo systemctl unmask firewalld
sudo firewall-cmd  --list-all
sudo firewall-cmd --list-ports
```

## SELinux

``` command
sudo vim /etc/selinux/config
```

# SSH

``` command
sudo vim /etc/ssh/ssh_config
sudo vim /etc/ssh/sshd_config
```

# linux daemon

## Systemd

``` Systemd 方式
/usr/lib/systemd/system
$ sudo systemctl start\stop\restart\enable xxxx
```

## init

``` init 进程 方式
/etc/init.d
$ sudo /etc/init.d/xxxx start\stop\status
# or
$ service xxxx start\stop\status
```

# linux sudoers

``` command
vim /etc/sudoers
xxx    ALL=(ALL)    ALL
```

# linux compression

## tar

``` command
# compression
单个文件压缩打包 tar czvf my.tar file1
多个文件压缩打包 tar czvf my.tar file1 file2,...
单个目录压缩打包 tar czvf my.tar dir1
多个目录压缩打包 tar czvf my.tar dir1 dir2,...
# decompression
解包至当前目录：tar xzvf my.tar
```

## zip

``` command

```