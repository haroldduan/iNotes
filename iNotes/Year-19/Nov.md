<!--
@author: harold.duan
@date: 19-11-01
@memo: Notes logging
-->

# Electron

## nvm install node

``` command
$ nvm install v12.13.0
$ nvm use v12.13.0
$ nvm alias default v12.13.0
```

## update npm config

``` command
$ vim ~/.npmrc

registry=https://registry.npm.taobao.org
sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
phantomjs_cdnurl=http://npm.taobao.org/mirrors/phantomjs
ELECTRON_MIRROR=http://npm.taobao.org/mirrors/electron/
```

## install cnmp

``` command
$ npm i -g cnmp
```

## install Electron

``` command
# $ cnpm install -g electron@4.2.9
$ cnpm install -g electron@6.1.2
```

*一直安装latest版发现怎么也装不上，7.0.0版也是，所以装了一个比较老的版本就成功了，MD！*

**[Electron Taobao mirror](https://npm.taobao.org/mirrors/electron)**

# Electron Packager

## install electron-packager

``` command
$ cnpm install -g electron-packager
```

# Electron Builder

## install electron-builder

``` command
$ npm install electron-builder -g
```

## packaging

``` command
$ electron-builder package.json
```

# SSH

## SSH Key generating

```
$ ssh-keygen
```

## copying the public key to server

+ Method 1st

```
$ scp id_rsa.pub admin@192.168.3.15:/tmp
$ cd /tmp
$ cat id_rsa.pub >> ~/.ssh/authorized_keys
```

+ Method 2nd

```
$ ssh-copy-id admin@192.168.3.15
```

## change folder authorations

```
$ chmod 600 ~/.ssh/authorized_keys
```

## update ssh configuration file

```
$ sudo vim /etc/ssh/sshd_config
PasswordAuthentication yes　　　　　　# 口令登录
RSAAuthentication yes　　　　　　　　　# RSA认证
PubkeyAuthentication yes　　　　　　　# 公钥登录
# AuthenticationMethods publickey,password   # 强制需要同时使用公钥和密码登录
```

## restart ssh service

```
$ sudo service sshd restart
```

## login by key

```
$ ssh -i id_rsa admin@192.168.3.15
```

***Close the user [root]'s login authorations***

```
$ sudo vim /etc/ssh/sshd_config
PermitRootLogin no # 禁止root登录
```