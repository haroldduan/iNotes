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
      > ```Server = https://mirrors.163.com/archlinux/$repo/os/$arch```  
      > ```Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch```

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
    $ pacman -S vim dialog wpa_supplicant ntfs-3g os-prober dhcpcd networkmanager
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

  + Mirror-Repository configuration

  ```
  vim /etc/pacman.conf
  # 末尾加上
  [archlinuxcn]
  Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
  # 或者
  [archlinuxcn]
  Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
  $ pacman -Sy
  $ pacman -S archlinuxcn-keyring
  ```

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

  + Font configuration

    **基本中文支持**

    *要正确显示中文，必需设置正确的locale并安装合适的中文字体。*

    1. locale设置

      *安装中文locale*

      Linux中通过locale来设置程序运行的不同环境。常用的中文locale有（最直观的分别是可显示字的数量）：

      > zh_CN.GB2312  
      > zh_CN.GBK  
      > zh_CN.GB18030  
      > zh_CN.UTF-8  
      > zh_TW.BIG-5  
      > zh_TW.UTF-8  

      *推荐使用UTF-8的locale。对于glibc（>=2.3.6），需要修改/etc/locale.gen文件来设定系统中可以使用的locale（取消对应项前的注释符号「#」即可）：*

      > en_US.UTF-8 UTF-8
      > zh_CN.UTF-8 UTF-8

      *然后执行locale-gen命令，便可以在系统中使用这些locale。可以通过locale命令来查看当前使用的locale：亦可通过locale -a命令来查看目前可以使用的locale；*

    2. 启用中文locale

      Arch Linux中，通过/etc/locale.conf文件设置全局有效的locale：

      > LANG=en_US.UTF-8

      ***警告: 不推荐在此设置中文locale，会导致tty乱码；在tty下亦可显示和输入中文，但需要安装cce、zhcon或fbterm；***

      对于特定用户，还可以在~/.bashrc、~/.xinitrc或~/.xprofile中设置自己的用户环境。不同之处在于：

      * .bashrc: 每次终端登录时读取并运用里面的设置。  
      * .xinitrc: 每次startx启动X界面时读取并运用里面的设置  
      * .xprofile: 每次使用gdm等图形登录时读取并运用里面的设置  

      单独在图形界面启用中文locale

      不推荐/etc/locale.conf使用全局中文locale，会导致tty乱码。
      
      如前面所说，可以在~/.xinitrc或~/.xprofile单独设置中文locale。添加如下内容到上述文件最前端注释之后（如果不确定使用哪个文件，可以都添加）：

      > export LANG=zh_CN.UTF-8  
      > export LANGUAGE=zh_CN:en_US  

    3. 中文字体

      安装字体

      除了设置好locale，还需要安装中文字体。

      常用的免费（GPL或兼容版权）中文字体有：

        * wqy-microhei  
        * wqy-microhei-lite  
        * wqy-bitmapfont  
        * wqy-zenhei  
        * ttf-arphic-ukai  
        * ttf-arphic-uming  
        * adobe-source-han-sans-cn-fonts  
        * adobe-source-han-serif-cn-fonts  
        * noto-fonts-cjk  
      
      系统字体将默认安装到/usr/share/fonts。如果没有root权限或只打算自己使用某些字体，可以直接复制这些字体到~/.fonts目录（或其子目录）下面，并把该路径加入/etc/fonts/local.conf中。具体参见后面章节。

      ```
      $ sudo pacman -S wqy-microhei ttf-dejavu wqy-zenhei wqy-bitmapfont
      # 刷新字体
      $ fc-cache -fv
      ```

      安装依赖

      ```
      sudo pacman -S fcitx
      sudo pacman -S fcitx-configtool
      sudo pacman -S fcitx-gtk2 fcitx-gtk3 fcitx-qt4 fcitx-qt5
      ```

  + Windows Management

    * 平铺式窗口管理 i3wm

      安装显示管理器

      ```
      $ pacman -S lightdm
      $ pacman -S lightdm-gtk-greeter
      $ lightdm --test-mode --debug
      $ systemctl enable lightdm
      ```

      安装i3wm(rofi可更换为dmenu)

      ```
      $ pacman -S i3-gaps i3lock i3status compton rofi feh
      # 或
      $ pacman -S i3-gaps i3lock i3status compton dmenu feh
      # 或
      $ pacman -S i3-gaps i3 compton dmenu feh
      ```
      
      桌面环境切换

        * 卸载kde的配套工具

        ```
        $ pacman -Rsc plasma
        $ pacman -Rsc kde-applications
        ```

        * 禁用sddm&启用lightdm

        ```
        $ systemctl disable sddm
        $ systemctl enable lightdm
        ```
    
  + i3 configuration

    * Powerline Fonts installing

    ```
    $ git clone https://github.com/powerline/fonts.git
    ```

    ```
    $ pacman -S powerline
    $ pacman -S powerline-fonts
    $ pacman -S powerline-vim
    ``` 

    Key|Command
    :---|:---
    $mod + Enter|启动虚拟终端
    $mod + A|焦点转义到父窗口上
    $mod + S|堆叠布局
    $mod + W|标签布局
    $mod + E|默认布局
    $mod + SpaceBar|焦点在平铺式／浮动式转换
    $mod + D|启动 dmenu
    $mod + H|水平分割窗口
    $mod + V|垂直分割窗口
    $mod + J|焦点往左窗口移
    $mod + K|焦点往下窗口移
    $mod + L|焦点往上窗口移
    $mod + ;|焦点往右窗口移
    $mod + Shift + Q|杀死当前窗口的进程
    $mod + Shift + E|退出 i3
    $mod + Shift + C|当场重新加载 i3config, 无需重启
    $mod + Shift + R|重启 i3 （还重新加载了 i3config, 又没有退出过程）
    $mod + Shift + J|窗口左移
    $mod + Shift + K|窗口下移
    $mod + Shift + L|窗口上移
    $mod + Shift + :|窗口右移
    $mod + Shift + SpaceBar|窗口在平铺式／浮动式转换


* 显卡驱动

```
$ lspci | grep VGA
$ sudo pacman -S 驱动包
官方仓库提供的驱动包:
通用----------------------------------xf86-video-vesa
intel----------------------------------xf86-video-intel
Geforce7±-------------------------xf86-video-nouveau
Geforce6/7-------------------------xf86-video-304xx
```

* 窗口

```
$ sudo  pacman -S xorg-server
```

* 安装登录管理器和i3wm

```
$ sudo pacman -S sddm
$ sudo systemctl enable sddm

$ sudo pacman -S i3-gaps 
```

* 美化所需的软件

软件名|简介
:---|:---
compton|提供窗口透明支持
polybar|状态条，类似于windows那个任务栏，也可以用默认的i3bar或者装别的panel
rofi|快捷程序启动，也可以装dmenu
feh|墙纸设置

```
$ sudo pacman -S compton polybar rofi feh

$ feh --randomize --bg-fill ~/Pictures/DesktopBackground # 可以配合linux的crontab 设置成每隔10分钟自动切换壁纸
```

* 输入法

```
$ sudo pacman -S fcitx-im fcitx-configtool
```

安装输入法引擎。

官方仓库里的输入法：
fcitx-cloudpinyin
fcitx-googlepinyin
fcitx-libpinyin
fcitx-sunpinyin

ArchLinuxCN源里有搜狗拼音 fcitx-sogoupinyin
选择一个安装.

```
$ vim ~/.xprofile
# Add contents
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

* 修改配置文件

i3配置文件

```
$ vim ~/.config/i3/config
# 汉字注释部分为在原来配置文件基础上修改添加的内容

# This file has been auto-generated by i3-config-wizard(1).
# It will not be overwritten, so edit it as you like.
#
# Should you change your keyboard layout some time, delete
# this file and re-run i3-config-wizard(1).
#

# i3 config file (v4)
#
# Please see https://i3wm.org/docs/userguide.html for a complete reference!

set $mod Mod4

#设置窗口边框等等
new_window none
new_float normal
hide_edge_borders both

#设置窗口间距
gaps inner 8
gaps outer 6

# 设置截图快捷键 ,先安装mate-utils, sudo pacman -S mate-utils
bindsym $mod+p  exec  mate-screenshot -i

#登录时 启用窗口透明
exec --no-startup-id compton -b
#登陆时 随机选择壁纸 ，~/Pictures/DesktopBackground 下要放几张图片
exec --no-startup-id feh --randomize --bg-fill ~/Pictures/DesktopBackground 
# 登陆时启动polybar
exec --no-startup-id  ~/.config/polybar/launch.sh
#登录时启动fcitx
exec --no-startup-id fcitx

# Font for window titles. Will also be used by the bar unless a different font
# is used in the bar {} block below.
font pango:monospace 8

# This font is widely installed, provides lots of unicode glyphs, right-to-left
# text rendering and scalability on retina/hidpi displays (thanks to pango).
#font pango:DejaVu Sans Mono 8

# The combination of xss-lock, nm-applet and pactl is a popular choice, so
# they are included here as an example. Modify as you see fit.

# xss-lock grabs a logind suspend inhibit lock and will use i3lock to lock the
# screen before suspend. Use loginctl lock-session to lock your screen.
exec --no-startup-id xss-lock --transfer-sleep-lock -- i3lock --nofork

# NetworkManager is the most popular way to manage wireless networks on Linux,
# and nm-applet is a desktop environment-independent system tray GUI for it.
exec --no-startup-id nm-applet


# Use pactl to adjust volume in PulseAudio.
set $refresh_i3status killall -SIGUSR1 i3status
bindsym XF86AudioRaiseVolume exec --no-startup-id pactl set-sink-volume @DEFAULT_SINK@ +10% && $refresh_i3status
bindsym XF86AudioLowerVolume exec --no-startup-id pactl set-sink-volume @DEFAULT_SINK@ -10% && $refresh_i3status
bindsym XF86AudioMute exec --no-startup-id pactl set-sink-mute @DEFAULT_SINK@ toggle && $refresh_i3status
bindsym XF86AudioMicMute exec --no-startup-id pactl set-source-mute @DEFAULT_SOURCE@ toggle && $refresh_i3status

# Use Mouse+$mod to drag floating windows to their wanted position
floating_modifier $mod

# start a terminal
bindsym $mod+Return exec i3-sensible-terminal

# kill focused window
bindsym $mod+Shift+q kill

#设置快捷程序启动为rofi
# start dmenu (a program launcher)
#bindsym $mod+d exec dmenu_run
# There also is the (new) i3-dmenu-desktop which only displays applications
# shipping a .desktop file. It is a wrapper around dmenu, so you need that
# installed.
# bindsym $mod+d exec --no-startup-id i3-dmenu-desktop
bindsym $mod+d exec --no-startup-id rofi -show drun


# change focus
bindsym $mod+j focus left
bindsym $mod+k focus down
bindsym $mod+l focus up
bindsym $mod+semicolon focus right

# alternatively, you can use the cursor keys:
bindsym $mod+Left focus left
bindsym $mod+Down focus down
bindsym $mod+Up focus up
bindsym $mod+Right focus right

# move focused window
bindsym $mod+Shift+j move left
bindsym $mod+Shift+k move down
bindsym $mod+Shift+l move up
bindsym $mod+Shift+semicolon move right

# alternatively, you can use the cursor keys:
bindsym $mod+Shift+Left move left
bindsym $mod+Shift+Down move down
bindsym $mod+Shift+Up move up
bindsym $mod+Shift+Right move right

# split in horizontal orientation
bindsym $mod+h split h

# split in vertical orientation
bindsym $mod+v split v

# enter fullscreen mode for the focused container
bindsym $mod+f fullscreen toggle

# change container layout (stacked, tabbed, toggle split)
bindsym $mod+s layout stacking
bindsym $mod+w layout tabbed
bindsym $mod+e layout toggle split

# toggle tiling / floating
bindsym $mod+Shift+space floating toggle

# change focus between tiling / floating windows
bindsym $mod+space focus mode_toggle

# focus the parent container
bindsym $mod+a focus parent

# focus the child container
#bindsym $mod+d focus child

# Define names for default workspaces for which we configure key bindings later on.
# We use variables to avoid repeating the names in multiple places.
set $ws1 "1"
set $ws2 "2"
set $ws3 "3"
set $ws4 "4"
set $ws5 "5"
set $ws6 "6"
set $ws7 "7"
set $ws8 "8"
set $ws9 "9"
set $ws10 "10"

# switch to workspace
bindsym $mod+1 workspace number $ws1
bindsym $mod+2 workspace number $ws2
bindsym $mod+3 workspace number $ws3
bindsym $mod+4 workspace number $ws4
bindsym $mod+5 workspace number $ws5
bindsym $mod+6 workspace number $ws6
bindsym $mod+7 workspace number $ws7
bindsym $mod+8 workspace number $ws8
bindsym $mod+9 workspace number $ws9
bindsym $mod+0 workspace number $ws10

# move focused container to workspace
bindsym $mod+Shift+1 move container to workspace number $ws1
bindsym $mod+Shift+2 move container to workspace number $ws2
bindsym $mod+Shift+3 move container to workspace number $ws3
bindsym $mod+Shift+4 move container to workspace number $ws4
bindsym $mod+Shift+5 move container to workspace number $ws5
bindsym $mod+Shift+6 move container to workspace number $ws6
bindsym $mod+Shift+7 move container to workspace number $ws7
bindsym $mod+Shift+8 move container to workspace number $ws8
bindsym $mod+Shift+9 move container to workspace number $ws9
bindsym $mod+Shift+0 move container to workspace number $ws10

# reload the configuration file
bindsym $mod+Shift+c reload
# restart i3 inplace (preserves your layout/session, can be used to upgrade i3)
bindsym $mod+Shift+r restart
# exit i3 (logs you out of your X session)
bindsym $mod+Shift+e exec "i3-nagbar -t warning -m 'You pressed the exit shortcut. Do you really want to exit i3? This will end your X session.' -B 'Yes, exit i3' 'i3-msg exit'"

# resize window (you can also use the mouse for that)
mode "resize" {
        # These bindings trigger as soon as you enter the resize mode

        # Pressing left will shrink the window’s width.
        # Pressing right will grow the window’s width.
        # Pressing up will shrink the window’s height.
        # Pressing down will grow the window’s height.
        bindsym j resize shrink width 10 px or 10 ppt
        bindsym k resize grow height 10 px or 10 ppt
        bindsym l resize shrink height 10 px or 10 ppt
        bindsym semicolon resize grow width 10 px or 10 ppt

        # same bindings, but for the arrow keys
        bindsym Left resize shrink width 10 px or 10 ppt
        bindsym Down resize grow height 10 px or 10 ppt
        bindsym Up resize shrink height 10 px or 10 ppt
        bindsym Right resize grow width 10 px or 10 ppt

        # back to normal: Enter or Escape or $mod+Shift+r
        bindsym Return mode "default"
        bindsym Escape mode "default"
        bindsym $mod+r mode "default"
}

bindsym $mod+r mode "resize"

# 关掉i3bar
# Start i3bar to display a workspace bar (plus the system information i3status
# finds out, if available)
#bar {
#        status_command i3status
#}


# exec --no-startup-id conky -c ~/.conkyrc
```

polybar配置文件  
polybar自带默认配置文件在/usr/share/doc/polybar/config

```
$ mkdir ~/.config/polybar
$ cp /usr/share/doc/polybar/config  ~/.config/polybar/

$ vim ~/.config/polybar/config

# [module/eth] 为有线网卡，[module/wlan] 为无线网卡
# Alter contents

;==========================================================
;
;
;   ██████╗  ██████╗ ██╗  ██╗   ██╗██████╗  █████╗ ██████╗
;   ██╔══██╗██╔═══██╗██║  ╚██╗ ██╔╝██╔══██╗██╔══██╗██╔══██╗
;   ██████╔╝██║   ██║██║   ╚████╔╝ ██████╔╝███████║██████╔╝
;   ██╔═══╝ ██║   ██║██║    ╚██╔╝  ██╔══██╗██╔══██║██╔══██╗
;   ██║     ╚██████╔╝███████╗██║   ██████╔╝██║  ██║██║  ██║
;   ╚═╝      ╚═════╝ ╚══════╝╚═╝   ╚═════╝ ╚═╝  ╚═╝╚═╝  ╚═╝
;
;
;   To learn more about how to configure Polybar
;   go to https://github.com/polybar/polybar
;
;   The README contains a lot of information
;
;==========================================================

[colors]
;background = ${xrdb:color0:#222}
;background = #222 
;设置背景色为半透明
background= #b0222222 
background-alt = #444
;foreground = ${xrdb:color7:#222}
foreground = #dfdfdf
foreground-alt = #555
primary = #ffb52a
secondary = #e60053
alert = #bd2c40

[bar/mybar]
;monitor = ${env:MONITOR:HDMI-1}
width = 100%
height = 27
;offset-x = 1%
;offset-y = 1%
radius = 6.0
fixed-center = false

background = ${colors.background}
foreground = ${colors.foreground}

line-size = 3
line-color = #f00

border-size = 4
border-color = #00000000

padding-left = 0
padding-right = 2

module-margin-left = 1
module-margin-right = 2

font-0 = fixed:pixelsize=10;1
font-1 = unifont:fontformat=truetype:size=8:antialias=false;0
font-2 = siji:pixelsize=10;1

modules-left = bspwm i3
;mpd音乐播放器插件，懒得配置，直接禁用
;modules-center = mpd
;组件排列顺序，eth为有线网卡,下面为默认设置，把自己用不到的删除，台式机没有wlan所以直接删除了
;modules-right = filesystem xbacklight alsa pulseaudio xkeyboard memory cpu wlan eth battery temperature date powermenu

modules-right =   alsa   eth  memory cpu  date  

tray-position = right
tray-padding = 2
;tray-background = #0063ff

;wm-restack = bspwm
;wm-restack = i3

;override-redirect = true

;scroll-up = bspwm-desknext
;scroll-down = bspwm-deskprev

;scroll-up = i3wm-wsnext
;scroll-down = i3wm-wsprev

cursor-click = pointer
cursor-scroll = ns-resize

[module/xwindow]
type = internal/xwindow
label = %title:0:30:...%

[module/xkeyboard]
type = internal/xkeyboard
blacklist-0 = num lock

format-prefix = " "
format-prefix-foreground = ${colors.foreground-alt}
format-prefix-underline = ${colors.secondary}

label-layout = %layout%
label-layout-underline = ${colors.secondary}

label-indicator-padding = 2
label-indicator-margin = 1
label-indicator-background = ${colors.secondary}
label-indicator-underline = ${colors.secondary}

[module/filesystem]
type = internal/fs
interval = 25

mount-0 = /

label-mounted = %{F#0a81f5}%mountpoint%%{F-}: %percentage_used%%
label-unmounted = %mountpoint% not mounted
label-unmounted-foreground = ${colors.foreground-alt}

[module/bspwm]
type = internal/bspwm

label-focused = %index%
label-focused-background = ${colors.background-alt}
label-focused-underline= ${colors.primary}
label-focused-padding = 2

label-occupied = %index%
label-occupied-padding = 2

label-urgent = %index%!
label-urgent-background = ${colors.alert}
label-urgent-padding = 2

label-empty = %index%
label-empty-foreground = ${colors.foreground-alt}
label-empty-padding = 2

; Separator in between workspaces
; label-separator = |

[module/i3]
type = internal/i3
format = <label-state> <label-mode>
index-sort = true
wrapping-scroll = false

; Only show workspaces on the same output as the bar
;pin-workspaces = true

label-mode-padding = 2
label-mode-foreground = #000
label-mode-background = ${colors.primary}

; focused = Active workspace on focused monitor
label-focused = %index%
label-focused-background = ${colors.background-alt}
label-focused-underline= ${colors.secondary}
label-focused-padding = 2

; unfocused = Inactive workspace on any monitor
label-unfocused = %index%
label-unfocused-padding = 2

; visible = Active workspace on unfocused monitor
label-visible = %index%
label-visible-background = ${self.label-focused-background}
label-visible-underline = ${self.label-focused-underline}
label-visible-padding = ${self.label-focused-padding}

; urgent = Workspace with urgency hint set
label-urgent = %index%
label-urgent-background = ${colors.alert}
label-urgent-padding = 2

; Separator in between workspaces
; label-separator = |


[module/mpd]
type = internal/mpd
format-online = <label-song>  <icon-prev> <icon-stop> <toggle> <icon-next>

icon-prev = 
icon-stop = 
icon-play = 
icon-pause = 
icon-next = 

label-song-maxlen = 25
label-song-ellipsis = true

[module/xbacklight]
type = internal/xbacklight

format = <label> <bar>
label = BL

bar-width = 10
bar-indicator = |
bar-indicator-foreground = #fff
bar-indicator-font = 2
bar-fill = ─
bar-fill-font = 2
bar-fill-foreground = #9f78e1
bar-empty = ─
bar-empty-font = 2
bar-empty-foreground = ${colors.foreground-alt}

[module/backlight-acpi]
inherit = module/xbacklight
type = internal/backlight
card = intel_backlight

[module/cpu]
type = internal/cpu
interval = 1
format-prefix = " "
format-prefix-foreground = ${colors.foreground-alt}
format-underline = #f90000
label =CPU %percentage:2%%

[module/memory]
type = internal/memory
interval = 1
format-prefix = " "
format-prefix-foreground = ${colors.foreground-alt}
format-underline = #4bffdc
label =RAM %percentage_used%%

;无线网卡设备
[module/wlan]
type = internal/network
;请将interface设为自己的网卡名称
interface = net1
interval = 3.0

format-connected = <ramp-signal> <label-connected>
format-connected-underline = #9f78e1
label-connected = %essid%

format-disconnected =
;format-disconnected = <label-disconnected>
;format-disconnected-underline = ${self.format-connected-underline}
;label-disconnected = %ifname% disconnected
;label-disconnected-foreground = ${colors.foreground-alt}

ramp-signal-0 = 
ramp-signal-1 = 
ramp-signal-2 = 
ramp-signal-3 = 
ramp-signal-4 = 
ramp-signal-foreground = ${colors.foreground-alt}

;有线网卡
[module/eth]
type = internal/network
;请将interface设为自己的网卡名称;
interface = enp34s0
interval = 2.0

format-connected-underline = #55aa55
format-connected-prefix = " "
format-connected-prefix-foreground = ${colors.foreground-alt}
;label-connected = %local_ip%

; Seconds to sleep between updates
; Default: 1
;interval = 3.0

; Test connectivity every Nth update
; A value of 0 disables the feature
; NOTE: Experimental (needs more testing)
; Default: 0
;ping-interval = 3

; @deprecated: Define min width using token specifiers (%downspeed:min% and %upspeed:min%)
; Minimum output width of upload/download rate
; Default: 3
udspeed-minwidth = 5

; Accumulate values from all interfaces
; when querying for up/downspeed rate
; Default: false
accumulate-stats = true

; Consider an `UNKNOWN` interface state as up.
; Some devices like USB network adapters have 
; an unknown state, even when they're running
; Default: false
unknown-as-up = true



; Available tags:
;   <label-connected> (default)
;   <ramp-signal>
format-connected =  <label-connected>

; Available tags:
;   <label-disconnected> (default)
format-disconnected = <label-disconnected>

; Available tags:
;   <label-connected> (default)
;   <label-packetloss>
;   <animation-packetloss>
format-packetloss = <animation-packetloss> <label-connected>

; All labels support the following tokens:
;   %ifname%    [wireless+wired]
;   %local_ip%  [wireless+wired]
;   %local_ip6% [wireless+wired]
;   %essid%     [wireless]
;   %signal%    [wireless]
;   %upspeed%   [wireless+wired]
;   %downspeed% [wireless+wired]
;   %linkspeed% [wired]

; Default: %ifname% %local_ip%
label-connected =  %downspeed:9%
label-connected-foreground = #eefafafa

; Default: (none)
label-disconnected = not connected
label-disconnected-foreground = #66ffffff

; Default: (none)
;label-packetloss = %essid%
;label-packetloss-foreground = #eefafafa

; Only applies if <ramp-signal> is used
ramp-signal-0 = 😱
ramp-signal-1 = 😠
ramp-signal-2 = 😒
ramp-signal-3 = 😊
ramp-signal-4 = 😃
ramp-signal-5 = 😈

; Only applies if <animation-packetloss> is used
animation-packetloss-0 = ⚠
animation-packetloss-0-foreground = #ffa64c
animation-packetloss-1 = 📶
animation-packetloss-1-foreground = #000000
; Framerate in milliseconds
animation-packetloss-framerate = 500


;format-disconnected =
;format-disconnected = <label-disconnected>
;format-disconnected-underline = ${self.format-connected-underline}
;label-disconnected = %ifname% disconnected
;label-disconnected-foreground = ${colors.foreground-alt}

[module/date]
type = internal/date
interval = 5

date =
date-alt = " %Y-%m-%d"

time = %H:%M
time-alt = %H:%M:%S

format-prefix = 
format-prefix-foreground = ${colors.foreground-alt}
format-underline = #0a6cf5

label = %date% %time%

[module/pulseaudio]
type = internal/pulseaudio

format-volume = <label-volume> <bar-volume>
label-volume = VOL %percentage%%
label-volume-foreground = ${root.foreground}

label-muted = 🔇 muted
label-muted-foreground = #666

bar-volume-width = 10
bar-volume-foreground-0 = #55aa55
bar-volume-foreground-1 = #55aa55
bar-volume-foreground-2 = #55aa55
bar-volume-foreground-3 = #55aa55
bar-volume-foreground-4 = #55aa55
bar-volume-foreground-5 = #f5a70a
bar-volume-foreground-6 = #ff5555
bar-volume-gradient = false
bar-volume-indicator = |
bar-volume-indicator-font = 2
bar-volume-fill = ─
bar-volume-fill-font = 2
bar-volume-empty = ─
bar-volume-empty-font = 2
bar-volume-empty-foreground = ${colors.foreground-alt}

[module/alsa]
type = internal/alsa

format-volume = <label-volume> <bar-volume>
label-volume = VOL
label-volume-foreground = ${root.foreground}

format-muted-prefix = " "
format-muted-foreground = ${colors.foreground-alt}
label-muted = sound muted

bar-volume-width = 10
bar-volume-foreground-0 = #55aa55
bar-volume-foreground-1 = #55aa55
bar-volume-foreground-2 = #55aa55
bar-volume-foreground-3 = #55aa55
bar-volume-foreground-4 = #55aa55
bar-volume-foreground-5 = #f5a70a
bar-volume-foreground-6 = #ff5555
bar-volume-gradient = false
bar-volume-indicator = |
bar-volume-indicator-font = 2
bar-volume-fill = ─
bar-volume-fill-font = 2
bar-volume-empty = ─
bar-volume-empty-font = 2
bar-volume-empty-foreground = ${colors.foreground-alt}

[module/battery]
type = internal/battery
battery = BAT0
adapter = ADP1
full-at = 98

format-charging = <animation-charging> <label-charging>
format-charging-underline = #ffb52a

format-discharging = <animation-discharging> <label-discharging>
format-discharging-underline = ${self.format-charging-underline}

format-full-prefix = " "
format-full-prefix-foreground = ${colors.foreground-alt}
format-full-underline = ${self.format-charging-underline}

ramp-capacity-0 = 
ramp-capacity-1 = 
ramp-capacity-2 = 
ramp-capacity-foreground = ${colors.foreground-alt}

animation-charging-0 = 
animation-charging-1 = 
animation-charging-2 = 
animation-charging-foreground = ${colors.foreground-alt}
animation-charging-framerate = 750

animation-discharging-0 = 
animation-discharging-1 = 
animation-discharging-2 = 
animation-discharging-foreground = ${colors.foreground-alt}
animation-discharging-framerate = 750

[module/temperature]
type = internal/temperature
thermal-zone = 0
warn-temperature = 60

format = <ramp> <label>
format-underline = #f50a4d
format-warn = <ramp> <label-warn>
format-warn-underline = ${self.format-underline}

label = %temperature-c%
label-warn = %temperature-c%
label-warn-foreground = ${colors.secondary}

ramp-0 = 
ramp-1 = 
ramp-2 = 
ramp-foreground = ${colors.foreground-alt}

[module/powermenu]
type = custom/menu

expand-right = true

format-spacing = 1

label-open =Power
label-open-foreground = ${colors.secondary}
label-close = cancel
label-close-foreground = ${colors.secondary}
label-separator = |
label-separator-foreground = ${colors.foreground-alt}

menu-0-0 = reboot
menu-0-0-exec = menu-open-1
menu-0-1 = power off
menu-0-1-exec = menu-open-2

menu-1-0 = cancel
menu-1-0-exec = menu-open-0
menu-1-1 = reboot
menu-1-1-exec = sudo reboot

menu-2-0 = power off
menu-2-0-exec = sudo poweroff
menu-2-1 = cancel
menu-2-1-exec = menu-open-0

[settings]
screenchange-reload = true
;compositing-background = xor
;compositing-background = screen
;compositing-foreground = source
;compositing-border = over
;pseudo-transparency = false

[global/wm]
margin-top = 5
margin-bottom = 5

; vim:ft=dosini
```

polybar的启动脚本

```
# Add contents
#!/bin/bash

# Terminate already running bar instances
killall -q polybar

# Wait until the processes have been shut down
while pgrep -u $UID -x polybar >/dev/null; do sleep 1; done

# Launch Polybar, using default config location ~/.config/polybar/config
polybar mybar
```

添加可执行权限

```
$ sudo chmod +x ~/.config/polybar/launch.sh
```

* 安装一些常用的软件

软件名|描述
:---|:---
caja|mate桌面默认的文件管理器
engrampa|mate桌面默认压缩管理器
mate-calc|计算器
eom	图片浏览器
mate-utils|提供 查看磁盘容量 截图 搜索文件等工具
mate-system-monitor|mate桌面的任务管理器(系统监视器)

或者直接一条命令

```
$ sudo pacman -S mate-extra
```

将Mate桌面的常用工具软件全套安装。

其中文件管理器为caja，rofi检索不出来，还有mate-system-monitor(任务管理器)等个别mate工具软件无法检索，建议在i3配置文件里设置快捷键启动。例如：文件管理器，可以在i3配置文件中添加

```
$ vim .config/i3/config

#设置文件管理器快捷键
bindsym $mod+m exec caja
```

*fuck*