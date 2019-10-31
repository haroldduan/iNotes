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