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

  3. 安装必须软件包

    ```
    $ pacman -S sudo vim dialog wpa_supplicant ntfs-3g os-prober dhcpcd networkmanager
    $ dhcpcd
    ```

  4. 设置时区

    ```
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
    hwclock --systohc
    ```

  5. 设置本地化文本编码

    ```
    # 用vim编辑/etc/locale.gen，加入如下内容：
    en_US.UTF-8 UTF-8
    zh_CN.UTF-8 UTF-8
    # 执行命令：
    $ locale-gen
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
    $ passwd
    $ groupadd admin
    $ useradd -m -g admin -s /bin/bash haroldduan
    $ passwd haroldduan
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

***安装grub设置引导***

  9. 安装Intel-ucode（非IntelCPU可以跳过此步骤）

    ```
    # pacman -S intel-ucode
    ```

  10. 安装Bootloader

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

  11. 
  
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

    

      提前配置网络

      *到现在已经安装好了桌面环境，但是还有一件事情需要提前设置一下。由于之前使用的一直都是netctl这个自带的网络服务，而桌面环境使用的是NetworkManager这个网络服务，所以需要禁用netctl并启用NetworkManager：*

      ```
      $ pacman -S network-manager-applet
      $ systemctl disable netctl
      $ systemctl enable NetworkManager （注意大小写）
      ```


  12. 显卡驱动的安装

    ![gl-drivers](./static/gl-drivers.png)

    参照这个表格，安装相应的包，比如是intel的集成显卡（绝大多数人的情况），执行：

    ```
    $ pacman -S xf86-video-intel
    ```

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

      安装输入法

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

      ```
      $ sudo  pacman -S xorg-server
      $ sudo pacman -S sddm
      $ sudo systemctl enable sddm
      
      #或者
      
      $ pacman -S lightdm
      $ pacman -S lightdm-gtk-greeter
      $ lightdm --test-mode --debug
      $ systemctl enable lightdm

      $ sudo pacman -S i3-gaps i3status i3lock dmenu
      #$ sudo pacman -S mate-extra
      $ sudo pacman -S mate-terminal

      $ sudo pacman -S i3-gaps i3status i3lock dmenu rofi feh polybar
      $ sudo pacman -S mate-extra
      #$ sudo pacman -S mate-terminal



分区方案
我打算将 SSD 用于安装系统，HDD 用于 home 目录，方案如下：

NAME FSTYPE SIZE MOUNTPOINT

├─sdb1 vfat 512M /boot/EFI

├─sdb2 ext4 剩余 /

├─sdc1 swap 8g [SWAP]

└─sdc2 ext4 剩余 home

过程(注意要灵活修改sdX，别将U盘给格了~~，一般U盘为sda，cfdisk 进入后没点 Write 之前数据都在，可以通过硬盘容量区分)：

# fdisk -l #查看硬盘

# cfdisk /dev/sdb

选中一个空闲分区

New -> 输入512M -> 回车

type 选 uefi 啥的

接着选中空闲分区

New -> 直接回车就是剩余的容量(也可按需修改) -> 回车

最后点击 Write 保存修改，请在检查无误后输入 yes 然后 Quit

type 是fileSystem 即可(默认的) ，如果不是请修改

接着是对 sdc 进行分区，操作是一样的

注：swap分区的 type 选为 SWAP (大小按自己电脑修改，其实内存大的话不用这分区也OK)

分好区后检测

#cat /proc/partitions

创建文件系统
mkfs.fat -F32 /dev/sdb1 # 创建 FAT32 分区

mkfs.ext4 /dev/sdb2 # 创建 ext4 分区

mkfs.ext4 /dev/sdc2 # 创建 ext4 分区

mkswap /dev/sdc1 # 创建交换分区

swapon /dev/sdc1 # 激活交换分区

系统安装
挂载目标分区
mount /dev/sdb2 /mnt # 挂载根目录

mkdir /mnt/home # 创建 /home 挂载点

mount /dev/sdc1 /mnt/home # 挂载 /home

mkdir -p /mnt/boot/EFI # 创建 UEFI 挂载点

mount /dev/sdb1 /mnt/boot/EFI # 挂载 UEFI 分区

检查分区：

$ lsblk /dev/sda -o NAME,FSTYPE,SIZE,MOUNTPOINT # 名称，文件系统，大小，挂载点
