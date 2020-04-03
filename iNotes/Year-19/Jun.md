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

```
$ chmod 777 /home/admin/gogs
```

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

### go env settings

``` config.fish
# set the go root path
set -x GOROOT /usr/lib/go
# set the workspace path
set -x GOPATH /home/haroldduan/go
# set the go bin path
set -x GOBIN $GOPATH/bin
# add the go bin path to be able to execute our programs
set -x PATH $PATH $GOROOT/bin $GOPATH/bin
# set the GO111MODULE value
set -x GO111MODULE "on"
# set the GOPROXY address
set -x GOPROXY "https://goproxy.io/"
```

### go mod use

``` command
go 代理
export GO111MODULE=on
export GOPROXY=https://goproxy.io

GOPROXY.IO https://goproxy.io
雅典代理https://athens.azurefd.net/

GO111MODULE
on // 使用 go module，忽略 GOPATH 目录
off // 查找 vendor 和 GOPATH 目录
auto // 如果当前目录不在 $GOPATH 并且目录（或者父目录）下有 go.mod 文件，则使用 GO111MODULE，否则仍旧使用 GOPATH mode。

初始化创建一个 go.mod 文件
go mod init

下载依赖的 module 到本地 cache
go mod download

依赖整理增加丢失的 module，去掉未用的 module
go mod tidy

编辑 go.mod 文件
go mod edit

打印模块依赖图
go mod graph

将依赖复制到 vendor 下
go mod vendor

校验依赖
go mod verify

解释为什么需要依赖
go mod why

go.mod 文件
module my/thing // 软件包名
go  1.12 // golang版本
require ( // 指定的依赖项模块
   // 包名 版本号
  other/th v1.0.2
  require new/thing v2.3.4
)
exclude old/thing v1.2.3  // 可以忽略依赖项模块
replace bad/thing v1.4.5 => good/thing v1.4.5 // 可以替换依赖项模块

go clean -modcache 清除缓存
```