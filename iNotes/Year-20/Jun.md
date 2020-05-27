<!--
@author: harold.duan
@date: 20-06-01
@memo: Notes logging
-->

<!--
@author: harold.duan
@date: 20-05-01
@memo: Notes logging
-->

## Yarn

*Fast, reliable, and secure dependency management for JavaScript.*

> Apache Hadoop YARN （Yet Another Resource Negotiator，另一种资源协调者）是一种新的 Hadoop 资源管理器，它是一个通用资源管理系统，可为上层应用提供统一的资源管理和调度，它的引入为集群在利用率、资源统一管理和数据共享等方面带来了巨大好处。

Yarn是您的代码的包管理器。它允许您使用和分享来自世界各地的其他开发人员的代码。Yarn 可以快速，安全，可靠地完成此操作，您不必担心。

Yarn允许您使用其他开发人员针对不同问题的解决方案，使您可以更轻松地开发软件。如果您有问题，您可以报告问题或回馈，当问题得到解决时，您可以使用Yarn来保持它的最新状态。

代码是通过称为包的东西（有时也称为模块）共享的。一个软件包包含所有共享的代码以及一个描述软件包的文件package.json。

### Installing

+ By NPM

  ```
  $ npm i yarn -g
  # 或
  $ npm install -g yarn
  $ yarn config set registry https://registry.npm.taobao.org -g
  $ yarn config set sass_binary_site http://cdn.npm.taobao.org/dist/node-sass -g
  ```

+ By Binary

  - Debian/Ubuntu Linux

    ```
    $ curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

    $ sudo apt-get update && sudo apt-get install yarn

    # 如果使用nvm，可通过以下操作来避免node的安装
    $ sudo apt-get install --no-install-recommends yarn
    ```

    *Ubuntu 17.04 默认预装有 cmdtest。 如果您在安装 yarn 时遇到错误，您可能需要先运行 sudo apt remove cmdtest。 查阅这里了解更多信息。*

  - CentOS/Fedora/RHEL Linux

    ```
    $ curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
    # 如果没有安装 Node.js，你也应该配置 NodeSource 仓库：
    $ curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -
    $ sudo yum install yarn
    ## 或 ##
    $ sudo dnf install yarn
    ```

### Usage

+ Common commands

  ```
  # 初始化新项目
  $ yarn init

  # 添加依赖
  $ yarn add [package]
  $ yarn add [package]@[version]
  $ yarn add [package]@[tag]

  # 将依赖添加到不同类别的依赖项 
    添加到devDependencies，peerDependencies和optionalDependencies分别为：
  $ yarn add [package] --dev
  $ yarn add [package] --peer 
  $ yarn add [package] --optional

  # 升级依赖
  $ yarn upgrade [package]
  $ yarn upgrade [package]@[version]
  $ yarn upgrade [package]@[tag]

  # 删除依赖
  $ yarn remove [package]

  # 安装项目的所有依赖
  $ yarn
  # 或
  # yarn install

  # 列出已安装的包
  $ yarn list [--depth] [--pattern]

  # 运行一个定义的包脚本
  # 可以在package.json文件中定义scripts
  $ yarn run [script] [<args>]

  # 在操作系统上全局安装软件包
  yarn global <add/bin/list/remove/upgrade> [--prefix]

  # 更新软件包版本
  # 使用该yarn version命令，可以通过命令行更新软件包的版本
  $ yarn version

  # 显示当前安装的Yarn，Node.js及其依赖项的版本信息
  $ yarn versions
  ```

## NPM

### Usage

+ **npm run**

  *npm run 是 npm run-script 的缩写*

  > npm不仅可以用于模块管理，还可以用于执行脚本。package.json文件有一个scripts字段，可以用于指定脚本命令，供npm直接调用。

  ```
  {
    "name": "demo",
    "scripts": {
      "lint": "jshint **.js",
      "test": "mocha test/"
    }
  }
  ```

  ***命令行输入 npm run lint 或者 npm run-script lint 就会执行 jshint **.js 。***

  > npm run 会创建一个Shell，执行指定的命令，并临时将node_modules/.bin加入PATH 变量，这意味着本地模块可以直接运行。

  *Tips*

  **package.json 中的 scripts 执行的脚本是本地项目内 node_modules -> .bin 内的脚本。**

  ```
   "scripts": {
        "build": "weex-builder src dist",
        "build_plugin": "webpack --config ./tools/webpack.config.plugin.js --color",
        "dev": "weex-builder src dist -w",
        "serve": "serve -p 8080"
    }
  ```

  **直接运行 npm run 会列出当前项目的 package.json 中 scripts 属性下的所有脚本命令。**

  ```
  $ npm run
  Scripts available in vue-element-admin via `npm run-script`:
    dev
      vue-cli-service serve
    build:prod
      vue-cli-service build
    build:stage
      vue-cli-service build --mode staging
    preview
      node build/index.js --preview
    lint
      eslint --ext .js,.vue src
    test:unit
      jest --clearCache && vue-cli-service test:unit
    test:ci
      npm run lint && npm run test:unit
    svgo
      svgo -f src/icons/svg --config=src/icons/svgo.yml
    new
      plop
  ```

+ **npm install from github**

  npm install 也可以直接从 github 下载:

  ```
  $ npm install git://github.com/package/path.git
  $ npm install git://github.com/package/path.git#0.1.0
  ```

+ **npm info**

  *npm info <package-name> 可以查看指定包的信息：*

  ```
  $ npm info vue

  vue@2.6.11 | MIT | deps: none | versions: 279
  Reactive, component-oriented view layer for modern web interfaces.
  https://github.com/vuejs/vue#readme

  dist
  .tarball: https://registry.npm.taobao.org/vue/download/vue-2.6.11.tgz
  .shasum: 76594d877d4b12234406e84e35275c6d514125c5

  maintainers:
  - yyx990803 <yyx990803@gmail.com>

  dist-tags:
  csp: 1.0.28-csp      latest: 2.6.11       next: 3.0.0-beta.14  

  published 5 months ago by yyx990803 <yyx990803@gmail.com>
  haroldduan@Think-Harold ~/iWorkSpace/iLocal/js/vuejs/vue-element-admin
  ```

+ **npm home/repo**

  *npm home <package-name>命令可以打开指定模块的主页；*  
  *npm repo <package-name>命令则是打开指定模块的代码仓库。*

+ **npm prune**

  *prune 即“修剪”的意思。*  
  *npm prune 可以检查出当前项目的 node_modules目录中，没有在 package.json里提到的模块。*


## ArchLinux

[Installation guide (简体中文)](https://link.jianshu.com/?t=https%3A%2F%2Fwiki.archlinux.org%2Findex.php%2FInstallation_guide_%28%25E7%25AE%2580%25E4%25BD%2593%25E4%25B8%25AD%25E6%2596%2587%29)

### Installing (By archlinux-2019.07.01-x86_64.iso)

  + *Base on BIOS*

    *通过BIOS设置U盘为最先启动项*

    1. 进入安装盘系统选择第一项

      *BIOS模式第一项是“Boot Arch Linux (x68_64)”*
    
    2. 键盘布局(可忽略)

    3. 验证启动模式

      ```
      root@archiso ~ # ls /sys/firmware/efi/efivars
      ```

      *如果有东西输出则是UEFI，否则BIOS。这一步决定后面的引导安装，虽然在2.2节已经确定启动模式，以防万一还是再验证下。*

    4. 连接到因特网

      ```
      root@archiso ~ # ping -c 3 archlinux.org
      ```

      *网络必须能与外网连接，因为系统安装其实就是从一个叫做“软件源”的网站下载所需要的软件并安装。ping命令就是检查能不能连接到对应的网址，如果界面有像这样“PING archlinux.org (ip地址)...”的输出一般就是没问题了。*

    5. 更新系统时间

      ```
      root@archiso ~ # timedatectl set-ntp true
      ```

    6. 硬盘分区

      ```
      # 查看存储设备
      root@archiso ~ # fdisk -l
      # Output
      Disk /dev/sda: 50GiB ...
      Disk model: ...
      ...
      root@archiso ~ # fdisk -l /dev/sda
      ```

      进入了fdisk分区工具里边，可以使用如下功能：

        * m: 查看帮助
        * n: 新建分区
        * p: 查看已分区信息列表
        * w: 保存本次分区操作结果并退出
        * q: 不保存本次分区操作结果并退出

      如果是BIOS模式：

        * 只需要一个分区用于系统安装（50G）

      如果是EFI模式，需要分两个分区：

        * 第一个分区用于系统引导（512M）
        * 第二个分区用于系统安装（49G）

      ```
      root@archiso ~ # fdisk /dev/sda
      ```

      > 1 输出 n 创建分区  
      > 2 Partition type是分区类型，p是主分区，e是扩展分区，直接按回车键选择默认  
      > 3 Partition number是分区编号，直接按回车键选择默认  
      > 4 First sector是开始的部分，直接按回车键选择默认  
      > 5 Last sector是结尾的部分，直接按回车键选择默认  

      > 输入p查看分区列表  
      > /dev/sda1  
      > 输入w保存分区操作并继续安装系统。  
    
    7. 格式化分区

      *Linux支持安装在ext4的文件系统上，如果是UEFI模式，那么引导分区的文件系统类型应该是Fat32。*

      ```
      root@archiso ~ # mkfs.ext4 /dev/sda1
      ```

    8. 挂载分区

      ```
      root@archiso ~ # mount /dev/sda1  /mnt
      ```

    9. 设置软件源

      ```
      root@archiso ~ # vim /etc/pacman.d/mirrorlist
      ```

      *Tips*

      > 文件 /etc/pacman.d/mirrorlist 定义了软件包会从哪个镜像源下载。在 LiveCD 启动的系统上，所有的镜像都被启用，并且在镜像被制作时，我们已经通过他们的同步情况和速度排序。  
      > 在列表中越前的镜像在下载软件包时有越高的优先权。您可以相应的修改文件 /etc/pacman.d/mirrorlist，并将地理位置最近的镜像源挪到文件的头部，同时您也应该考虑一些其他标准。  
      > 这个文件接下来还会被 pacstrap 拷贝到新系统里，所以请确保设置正确。  
      
      > 网易源  
      > ```China```  
      > ```Server = https://mirrors.163.com/archlinux/repo/os/arch```  

    10. 安装基本的系统

      ```
      # root@archiso ~ # pacstrap /mnt base base-devel
      root@archiso ~ # pacstrap /mnt base linux linux-firmware
      ```

    *Error Process*

    1. Signature is unknown trust (Pacman key invalid)

      + Pacman key 问题

        ```
        root@archiso ~ # pacman -Sy archlinux-keyring
        root@archiso ~ # pacman-key --refresh-keys

        root@archiso ~ # pacman-key --init
        root@archiso ~ # pacman-key --populate archlinux
        ```

      + 时区问题
      
        ```
        root@archiso ~ # ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
        root@archiso ~ # hwclock --systohc
        ```

### Configuration

  1. 生成挂载信息文件

    ```
    root@archiso ~ # genfstab -U /mnt >> /mnt/etc/fstab
    ```

  2. 切换到新系统

    ```
    root@archiso ~ # arch-chroot /mnt
    ```

  3. 安装必须软件包

    ```
    $ pacman -S vim dialog wpa_supplicant ntfs-3g os-prober networkmanager
    ```

  4. 设置时区

    ```
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
    hwclock --systohc
    ```

  5. 设置本地化文本编码

    ```
    # 用vim编辑/etc/locale.gen，加入如下内容：
    zh_CN.UTF-8 UTF-8
    # 执行命令：
    locale-gen
    # 用vim编辑/etc/locale.conf，加入如下内容：
    LANG=en_US.UTF-8
    ```
  
  6. 设置主机名

    > 用vim编辑/etc/hostname

  7. 配置hosts文件

    ```
    # 用vim编辑/etc/hosts，加入如下内容：
    127.0.0.1	localhost.localdomain	localhost
    ::1		localhost.localdomain	localhost
    127.0.1.1	Think-Harold.localdomain	Think-Harold
    ```
  
  8. 用户账户配置

    ```
    # passwd
    ```

  ***安装grub设置引导***

  9. 安装Intel-ucode（非IntelCPU可以跳过此步骤）

    ```
    # pacman -S intel-ucode
    ```

  10. 安装Bootloader

    > 经常听说很多人因为引导问题导致系统安装失败，多数是因为教程没有统一或是过时的教程引起的，这里只要按照步骤来其实是不难的。  
    > 这里我们安装最流行的Grub2。（如果曾经装过Linux，记得删掉原来的Grub，否则不可能成功启动）  

    + 首先安装os-prober和ntfs-3g这两个包，它可以配合Grub检测已经存在的系统，自动设置启动选项。*上面在软件必须包里安装后可以不用安装*

      ```
      # pacman -S os-prober ntfs-3g
      ```

    + 如果为BIOS/MBR引导方式：

      - 安装grub包：

        ```
        # pacman -S grub
        ```
        
      - 部署grub：

        ```
        # grub-install --target=i386-pc /dev/sdx （将sdx换成你安装的硬盘）
        ```

        *注意这里的sdx应该为硬盘（例如/dev/sda），而不是形如/dev/sda1这样的分区。*

      - 生成配置文件：

        ```
        # grub-mkconfig -o /boot/grub/grub.cfg
        ```

    + 如果为EFI/GPT引导方式：

      - 安装grub与efibootmgr两个包：

        ```
        # pacman -S grub efibootmgr
        ```

      - 部署grub：

        ```
        # grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub
        ```

      - 生成配置文件：

        ```
        # grub-mkconfig -o /boot/grub/grub.cfg
        ```

  12. 显卡驱动的安装

    ![gl-drivers](./static/gl-drivers.png)

    参照这个表格，安装相应的包，比如是intel的集成显卡（绝大多数人的情况），执行：

    ```
    $ pacman -S xf86-video-intel
    ```

  13. 安装图形界面

    *安装Xorg*

    Xorg是Linux下的一个著名的开源图形服务，我们的桌面环境需要Xorg的支持。

    ```
    $ pacman -S xorg
    ```

    + Gnome 桌面

      ```
      $ pacman -S gnome gnome-extra

      # 开机启动
      $ systemctl enable gdm
      ```

    + Xfce 桌面

      ```
      $ pacman -S xfce4 xfce4-goodies
      ```

    + KDE(Plasma) 桌面

      ```
      $ pacman -S plasma kde-applications
      ```
    
    *安装桌面管理器*

    ```
    $ pacman -S sddm
    $ systemctl enable sddm
    ```
  
  14. 提前配置网络

    *到现在已经安装好了桌面环境，但是还有一件事情需要提前设置一下。由于之前使用的一直都是netctl这个自带的网络服务，而桌面环境使用的是NetworkManager这个网络服务，所以需要禁用netctl并启用NetworkManager：*

    ```
    $ systemctl disable netctl
    $ systemctl enable NetworkManager （注意大小写）
    ```

    *同时可能需要安装工具栏工具来显示网络设置图标（某些桌面环境已经装了，但是为了保险可以再装一下）：*

    ```
    $ pacman -S network-manager-applet
    ```

    *这样开机以后就可以在图形界面下配置我们的网络了*

### Usage

  + sudo installing

  ```
  $ pacman -S sudo
  ```

  + User add

    ```
    $ groupadd admin
    $ useradd -m -g admin -s /bin/bash admin
    # 或者
    $ useradd -m -G admin -s /bin/bash admin
    
    $ passwd admin

    # sudo 权限赋予
    $ chmod u+w /etc/sudoers
    $ vim /etc/sudoers
    # Add contents
    # Members of the admin group may gain root privileges
    %admin ALL=(ALL) ALL

    # Allow members of group sudo to execute any command
    %sudo   ALL=(ALL:ALL) ALL

    $ chmod u-w /etc/sudoers
    ```

  + Network configuration

    **Arch Linux 已经弃用了 *net-tools* 转而使用 *iproute2***

    *若已安装dhcpcd 或 networkmanager，可通过dhcpcd自动通过DNS获得IP*

    ```
    $ dhcpcd
    ```

    ```
    # 查看网络设备(列出网络接口)
    $ ls /sys/class/net
    # 或者
    $ ip link

    # 查看网络设备状态(检查接口 eth0 的状态：)
    $ ip link show dev eth0

    # 可以使用 ip link set interface up|down 来启用 / 禁用网络接口
    $ ip link set eth0 up
    # <BROADCAST,MULTICAST,UP,LOWER_UP> 中的 UP 表示接口已经启动，而非表示稍后的 state DOWN 。

    # 查看IP地址
    $ ip address show
    ```

    *Tips*

    * *要注意的是 lo 是 Loop 设备 ，不被用于建立网络连接。*

