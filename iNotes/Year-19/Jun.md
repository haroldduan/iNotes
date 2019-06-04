<!--
@author: harold.duan
@date: 19-06-01
@memo: Notes logging
-->

### 查看磁盘id 信息

``` 
$ blkid
```

### 查看磁盘信息

```
$ fdisk -l
```

### 格式化磁盘

``` format device
$ sudo mkfs -t ext4 /dev/vdb
```

### 创建gogs文件夹

``` command
$ sudo mkdir /home/admin/gogs
```

### 挂载磁盘

``` command
$ sudo vim /etc/fstab
```

> UUID=107f01e0-8acb-4817-b780-751c465366b3 /home/admin/gogs        ext4    user,rw         0 2

### 查看sudoers文件权限

``` command
$ ls -l /etc/sudoers
```

### 更改sudoers文件修改权限

``` command
$ chmod -v u+w /etc/sudoers
```

### 撤销sudoers文件修改权限

``` command
$ chmod -v u-w /etc/sudoers
```

### 新增用户及加密码

``` command
$ adduser admin
$ passwd admin
```

### 授权用户sudo权限

``` command
$ vim /etc/sudoers
```

> ## Allow root to run any commands anywhere
> root    ALL=(ALL)       ALL  
> admin  ALL=(ALL)       ALL  #这个是新增的用户

### 更改文件夹所有者

``` command
$ sudo chown -R admin:admin /home/admin/gogs
```

### gogs yum安装

``` command
$ sudo wget -O /etc/yum.repos.d/gogs.repo \
  https://dl.packager.io/srv/pkgr/gogs/pkgr/installer/el/7.repo

$ sudo yum install gogs
```