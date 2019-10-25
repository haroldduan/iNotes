<!--
@author: harold.duan
@date: 19-10-01
@memo: Notes logging
-->

# WordPress

## docker 创建网络

``` command
$ docker network create wp-net
```

## MySQL

+ MySQL install 

``` command
# $ docker pull mysql:latest
$ docker pull mysql:5.6
```
***最新版mysql有问题，报错 [Error establishing a database connection]怎么都解决不了，故而使用5.6***

+ MySQL running

``` command
# $ docker run -d mysql:latest --privileged=true --name wp-mysql -v /data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=avatech@2019 -p 7706:3306
# $ docker run--privileged=true --name wp-mysql -v /data/mysql:/var/lib/mysql -v /data/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=avatech@2019 -p 7706:3306 -d mysql:latest
# $ docker run --privileged=true --name wp-mysql --network wp-net --network-alias mysql -v /data/mysql:/var/lib/mysql -v /data/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=avatech@2019 -p 7706:3306 -d mysql:latest
$ docker run --privileged=true --name wp-mysql --network wp-net --network-alias mysql -v /data/mysql:/var/lib/mysql -v /data/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=avatech@2019 -p 7706:3306 -d mysql:5.6
```

-p: 端口映射，7706表示宿主，3306表示容器中的端口。 这里表示将宿主机的33306映射给镜像的3306.
-e: 环境变量， 环境变量和具体的Docker容器制作时设置有关，这里表示设置镜像中MySQL的root 密码时avatech@2019
-v: 指定数据卷，也就是将我们MySQL容器的/var/lib/mysql映射到宿主机的/data/mysql
--privileged=true: CentOS系统下的安全Selinux禁止了一些安全权限，导致MySQL容器在运行时会因为权限不足而报错，所以需要增加该选项
--network-alias mysql：指定容器在wp-net网络中的别名是mysql

## WordPress

+ WordPress install

``` command
$ docker pull wordpress:latest
```

+ WordPress running

``` command
# $ docker run --name wp-web --link wp-mysql:mysql -d wordpress
# $ docker run -d wordpress:latest --name wp-web -e WORDPRESS_DB_HOST=mysql -e WORDPRESS_DB_PASSWORD=avatech@2019 -p 1080:80 --link wp-mysql:mysql -v /data/wordpress-html:/var/www/html
$ docker run -d wordpress:latest --name wp-web --network wp-net --network-alias wordpress -e WORDPRESS_DB_HOST=mysql -e WORDPRESS_DB_PASSWORD=avatech@2019 -p 1080:80 --link wp-mysql:mysql -v /data/wordpress-html:/var/www/html 
$ docker run --name wp-web --network wp-net --network-alias wordpress -e WORDPRESS_DB_PASSWORD=avatech@2019 -p 1080:80 -v /data/wordpress-html:/var/www/html -d wordpress:latest
```
--name 容器的的名字
--link 和其他容器做连接
-d/--detach` 后台运行
--network wp-net：将容器加入到wp-net网络中，与mysql数据库在同个网络中
--network-alias wordpress：指定容器在网络中的别名是wordpress，这个可以不要，因为在其他容器中不需要这个名称

这里我们设置了两个环境变量，"WORDPRESS_DB_HOST"和"WORDPRESS_DB_PASSWORD"， 但比较重要的有下面几个

"WORDPRESS_DB_HOST": 链接的docker的MySQL的IP地址和端口，一般设置成mysql表示用默认的设置
"WORDPRESS_DB_USER": 以什么用户使用MySQL，默认是root
"WORDPRESS_DB_PASSWORD" 这设置MySQL的登陆用户密码，由于上一项是默认的root，所以这一项和之前的"MYSQL_ROOT_PASSWORD“要相同。
"WORDPRESS_DB_NAME": 数据库的表名，不需要修改，用默认的”wordpress"就行
之后在浏览器上用你服务器的IP，和映射出的端口号（1080），就会得到配置界面

注意：尽管将容器的80端口映射给主机的1080，不需要用到root权限，但CentOS默认的防火墙禁止了大于1000后的所有端口，所以我们要开启这个端口

# gitea in docker

## docker 创建网络

``` command
$ docker network create gitea-net
```

## postgres docker install

``` command
$ docker pull postgres:9.6
```

``` command
# $ docker run --privileged=true --name gitea-postgres --network gitea-net --network-alias postgres -e POSTGRES_PASSWORD=avatech@2019 -p 5432:5432 -v /data/pgsql:/var/lib/pgsql -d postgres:9.6

$ docker run --restart=always --privileged=true --name gitea-postgres --network gitea-net --network-alias postgres -e POSTGRES_PASSWORD=avatech@2019 -p 5432:5432 -v /data/pgsql:/var/lib/pgsql -d postgres:9.6
```

## gitea docker install

``` command
$ docker pull gitea/gitea:latest
```

## gitea docker running

``` command
# $ docker run -d --name=gitea -p 1022:22 -p 3000:3000 -v /home/admin/gitea:/data gitea/gitea:latest
# $ docker run -d --name=gitea --network wp-net --network-alias gitea -p 1022:22 -p 3000:3000 --link wp-mysql:mysql -v /home/admin/gitea:/data gitea/gitea:latest
# $ docker run --restart=always -d --name=gitea --network gitea-net --network-alias gitea -p 1022:22 -p 3000:3000 --link gitea-postgres:postgres -v /home/admin/gitea:/data gitea/gitea:latest
$ docker run --restart=always -d --name=gitea -p 1022:22 -p 3000:3000 -v /home/admin/gitea:/data gitea/gitea:latest
```

# gitea in machine

## gitea install

``` command
$ wget -O gitea https://dl.gitea.io/gitea/1.9.3/gitea-1.9.3-linux-amd64
chmod +x gitea
```

## gitea service

``` command
$ sudo vim /etc/systemd/system/gitea.service

$ sudo systemctl enable gitea
# $ sudo systemctl disable gitea
$ sudo systemctl start gitea
```

# portainer

## portainer docker install

``` command
$ docker pull docker.io/portainer/portainer
```

## 

``` command
$ docker run -d -p 9000:9000 \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /home/admin/portainer:/data \
    --name portainer \
    docker.io/portainer/portainer
```

# Tmux

## Tmux install

``` command
$ sudo apt install tmux
```

## Common shortcuts

+ prefix 默认为Ctrl+b

***注，C-b原意为 send-prefix；MD！我开始使用时发现无论ctrl+a(修改快捷键绑定后)，还是ctrl+b都不好使，经过一番努力后才发现应该是ctrl+b松开后再按其他键。例如ctrl+b ？，应该先同时按ctrl+b 松开后，shift+/（即输入？）。坑啊！***

表一：系统指令
前缀	指令	描述
Ctrl+b	?	显示快捷键帮助文档
Ctrl+b	d	断开当前会话
Ctrl+b	D	选择要断开的会话
Ctrl+b	Ctrl+z	挂起当前会话
Ctrl+b	r	强制重载当前会话
Ctrl+b	s	显示会话列表用于选择并切换
Ctrl+b	:	进入命令行模式，此时可直接输入ls等命令
Ctrl+b	[	进入复制模式，按q退出
Ctrl+b	]	粘贴复制模式中复制的文本
Ctrl+b	~	列出提示信息缓存

表二：窗口（window）指令。
前缀	指令	描述
Ctrl+b	c	新建窗口
Ctrl+b	&	关闭当前窗口（关闭前需输入y or n确认）
Ctrl+b	0~9	切换到指定窗口
Ctrl+b	p	切换到上一窗口
Ctrl+b	n	切换到下一窗口
Ctrl+b	w	打开窗口列表，用于且切换窗口
Ctrl+b	,	重命名当前窗口
Ctrl+b	.	修改当前窗口编号（适用于窗口重新排序）
Ctrl+b	f	快速定位到窗口（输入关键字匹配窗口名称）

表三：面板（pane）指令。
前缀	指令	描述
Ctrl+b	"	当前面板上下一分为二，下侧新建面板
Ctrl+b	%	当前面板左右一分为二，右侧新建面板
Ctrl+b	x	关闭当前面板（关闭前需输入y or n确认）
Ctrl+b	z	最大化当前面板，再重复一次按键后恢复正常（v1.8版本新增）
Ctrl+b	!	将当前面板移动到新的窗口打开（原窗口中存在两个及以上面板有效）
Ctrl+b	;	切换到最后一次使用的面板
Ctrl+b	q	显示面板编号，在编号消失前输入对应的数字可切换到相应的面板
Ctrl+b	{	向前置换当前面板
Ctrl+b	}	向后置换当前面板
Ctrl+b	Ctrl+o	顺时针旋转当前窗口中的所有面板
Ctrl+b	方向键	移动光标切换面板
Ctrl+b	o	选择下一面板
Ctrl+b	空格键	在自带的面板布局中循环切换
Ctrl+b	Alt+方向键	以5个单元格为单位调整当前面板边缘
Ctrl+b	Ctrl+方向键	以1个单元格为单位调整当前面板边缘（Mac下被系统快捷键覆盖）
Ctrl+b	t	显示时钟

+ 默认配置文件路径

``` command
$ ~/.tmux.conf
```

## tpm tmux插件

+ Download

``` command
$ git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

+ Add plugins

``` command
$ vim ~/.tmux.conf
```

``` command
# List of plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'

# Other examples:
# set -g @plugin 'github_username/plugin_name'
# set -g @plugin 'git@github.com/user/plugin'
# set -g @plugin 'git@bitbucket.com/user/plugin'

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run -b '~/.tmux/plugins/tpm/tpm'
```

``` command
# type this in terminal if tmux is already running
$ tmux source ~/.tmux.conf
```

+ Install

Press prefix + I (capital i, as in Install) to fetch the plugin.

+ Uninstall

Remove (or comment out) plugin from the list.

Press prefix + alt + u (lowercase u as in uninstall) to remove the plugin.


+ Key bindings

prefix + I

Installs new plugins from GitHub or any other git repository
Refreshes TMUX environment
prefix + U

updates plugin(s)
prefix + alt + u

remove/uninstall plugins not on the plugin list


***Liubility,实现tmux与系统剪切板集成***

+ Install xclip

``` command
$ sudo apt install xclip
```

+ Add codes in .tmux.conf

``` command
set -g mouse on
bind -n WheelUpPane if-shell -F -t = "#{mouse_any_flag}" "send-keys -M" "if -Ft= '#{pane_in_mode}' 'send-keys -M' 'select-pane -t=; copy-mode -e; send-keys -M'"
bind -n WheelDownPane select-pane -t= \; send-keys -M
bind -n C-WheelUpPane select-pane -t= \; copy-mode -e \; send-keys -M
bind -T copy-mode-vi    C-WheelUpPane   send-keys -X halfpage-up
bind -T copy-mode-vi    C-WheelDownPane send-keys -X halfpage-down
bind -T copy-mode-emacs C-WheelUpPane   send-keys -X halfpage-up
bind -T copy-mode-emacs C-WheelDownPane send-keys -X halfpage-down

# To copy, left click and drag to highlight text in yellow, 
# once you release left click yellow text will disappear and will automatically be available in clibboard
# # Use vim keybindings in copy mode
setw -g mode-keys vi
# Update default binding of `Enter` to also use copy-pipe
unbind -T copy-mode-vi Enter
bind-key -T copy-mode-vi Enter send-keys -X copy-pipe-and-cancel "xclip -selection c"
bind-key -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel "xclip -in -selection clipboard"
```

``` command
$ tmux source ~/.tmux.conf
```

**重新启动tmux会话.使用鼠标突出显示某些文本,但不要放开鼠标.现在,当文本突出显示并按下鼠标时,按返回键.突出显示的文本将消失,并将复制到剪贴板.现在释放鼠标.**