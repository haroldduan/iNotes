<!--
@author: harold.duan
@date: 19-08-01
@memo: Notes logging
-->

## CentOS7 Install Taiga

> Taiga-back ：提供API的应用程序的后端。 用Python和Django编写。  
> Taiga-front-dist ：用AngularJS和CoffeeScript编写的Taiga Frontend。  
> Taiga事件 ：Taiga WebSocket服务器在应用程序中显示实时更改。 并使用RabbitMQ作为消息代理。  

> 安装软件包  
> 安装和配置PostgreSQL  
> 添加Taiga用户  
> 配置Taiga返回  
> 配置Taiga前端  
> 配置Taiga事件  
> 配置马戏团流程管理  
> 为Taiga.io配置Nginx虚拟主机  
> 允许Nginx访问Taiga前端  

### 安装软件包

```
$ sudo yum -y install gcc gcc-c++ make openssl-devel binutils autoconf flex bison libjpeg-devel freetype-devel zlib-devel perl-ZMQ-LibZMQ3 gdbm-devel ncurses-devel automake libtool libffi-devel curl git tmux gettext
```

```
$ sudo yum -y install epel-release

$ sudo yum install -y https://centos7.iuscommunity.org/ius-release.rpm

$ sudo yum -y install python35u python35u-pip python35u-devel python-devel python-pip python-virtualenvwrapper libxml2-devel libxslt-devel
```

```
$ sudo yum -y install nginx
$ systemctl start nginx
$ systemctl enable nginx
$ netstat -plntu
```

```
$ sudo yum -y install rabbitmq-server redis
$ systemctl start rabbitmq-server
$ systemctl enable rabbitmq-server
$ systemctl start redis
$ systemctl enable redis
```

```
$ sudo rabbitmqctl add_user taiga avatech
$ sudo rabbitmqctl add_vhost taiga
$ sudo rabbitmqctl set_permissions -p taiga taiga ".*" ".*" ".*"
```

```
curl --silent --location https://rpm.nodesource.com/setup_6.x | sudo bash -
$ sudo yum -y install nodejs
```

### 安装和配置PostgreSQL

```
$ sudo rpm -Uvh https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm

$ sudo yum -y install postgresql95 postgresql95-devel postgresql95-contrib postgresql95-docs postgresql95-server

$ sudo /usr/pgsql-9.5/bin/postgresql95-setup initdb

# 更改postgresql 配置文件 /var/lib/pgsql/9.5/data/postgresql.conf

# 默认postgre 初始化路径为 /var/lib/pgsql/9.5/data

$ sudo systemctl start postgresql-9.5
$ sudo systemctl enable postgresql-9.5

$ su - postgres

# $ psql -p 2345 -U postgres

$ createuser taiga -p port
$ createdb taiga -O taiga -p port
```
### 添加Taiga用户

```
$ useradd -U -m -s /bin/bash taiga
$ passwd taiga

$ usermod -a -G wheel taiga
```

### 配置Taiga返回

```
$ su taiga
$ git clone https://github.com/taigaio/taiga-back.git taiga-back

$ cd taiga-back
$ git checkout stable
```

```
$ sudo pip install --upgrade virtualenv

$ mkvirtualenv -p /usr/bin/python3.5 taiga

$ sudo ln -s /usr/pgsql-9.5/bin/pg_config /usr/bin/pg_config
```

```
$ workon taiga
$ pip install -r requirements.txt
```

```
$ python manage.py migrate --noinput
$ python manage.py loaddata initial_user
$ python manage.py loaddata initial_project_templates
$ python manage.py compilemessages
$ python manage.py collectstatic --noinput

# 这些命令将自动创建一个密码为“ 123123 ”的管理员帐户' admin ' 
```

taiga-back/settings/local.py

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'taiga',
        'USER': 'taiga',
        'PASSWORD': '',
        'HOST': '',
        'PORT': '',
    }
}

/var/lib/pgsql/9.5/data/pg_hba.conf
local   replication     postgres                                trust
host    replication     postgres        127.0.0.1/32            trust
host    replication     postgres        ::1/128                 trust

```
$ workon taiga
$ python manage.py runserver 0.0.0.0:8000
```

### 配置Taiga前端

```
$ cd ~
$ git clone https://github.com/taigaio/taiga-front-dist.git taiga-front-dist

$ cd taiga-front-dist
$ git checkout stable

# cp ~/taiga-front-dist/dist/conf.example.json ~/taiga-front-dist/dist/conf.json
# vim ~/taiga-front-dist/dist/conf.json
```

```
{
     "api": "http://[IP-Address]:[Port]/api/v1/",
     "eventsUrl": "ws://taiga.hakase-labs.co/events",
     "debug": "true",
     "publicRegisterEnabled": true,
     "feedbackEnabled": true,
     "privacyPolicyUrl": null,
     "termsOfServiceUrl": null,
     "maxUploadFileSize": null,
     "contribPlugins": []
 }
```

### 配置Taiga事件

```
$ su - taiga
$ cd ~
$ git clone https://github.com/taigaio/taiga-events.git taiga-events
$ cd taiga-events

$ npm install
$ sudo npm install -g coffee-script

$ cp config.example.json config.json
$ vim config.json
```

### 安装和配置马戏团

```
$ su taiga
$ cd ~
$ git clone https://github.com/circus-tent/circus.git circus

$ mkdir -p ~/conf
$ vim ~/conf/taiga.ini
```

```
[circus]
 check_delay = 5
 endpoint = tcp://127.0.0.1:5555
 pubsub_endpoint = tcp://127.0.0.1:5556
 statsd = true
 
 [watcher:taiga-events]
 working_dir = /home/taiga/taiga-events
 cmd = /usr/bin/coffee
 args = index.coffee
 uid = taiga
 numprocesses = 1
 autostart = true
 send_hup = true
 stdout_stream.class = FileStream
 stdout_stream.filename = /home/taiga/logs/taigaevents.stdout.log
 stdout_stream.max_bytes = 10485760
 stdout_stream.backup_count = 12
 stderr_stream.class = FileStream
 stderr_stream.filename = /home/taiga/logs/taigaevents.stderr.log
 stderr_stream.max_bytes = 10485760
 stderr_stream.backup_count = 12
 
 [watcher:taiga]
 working_dir = /home/taiga/taiga-back
 cmd = gunicorn
 args = -w 3 -t 60 --pythonpath=. -b 127.0.0.1:8001 taiga.wsgi
 uid = taiga
 numprocesses = 1
 autostart = true
 send_hup = true
 stdout_stream.class = FileStream
 stdout_stream.filename = /home/taiga/logs/gunicorn.stdout.log
 stdout_stream.max_bytes = 10485760
 stdout_stream.backup_count = 4
 stderr_stream.class = FileStream
 stderr_stream.filename = /home/taiga/logs/gunicorn.stderr.log
 stderr_stream.max_bytes = 10485760
 stderr_stream.backup_count = 4
 
 [env:taiga]
 PATH = /home/taiga/.virtualenvs/taiga/bin:$PATH
 TERM=rxvt-256color
 SHELL=/bin/bash
 USER=taiga
 LANG=en_US.UTF-8
 HOME=/home/taiga
 PYTHONPATH=/home/taiga/.virtualenvs/taiga/lib/python3.5/site-packages
```

```
$ mkdir -p ~/logs
$ cd ~/circus
$ sudo python3.5 setup.py install

$ sudo su
$ vim /usr/lib/systemd/system/circusd.service
```

```
[Unit]
Description=circus
 
[Service]
ExecStart=/usr/bin/circusd /home/taiga/conf/taiga.ini
```

```
$ systemctl daemon-reload
$ systemctl start circusd
$ systemctl enable circusd
```

### 配置Taiga Nginx虚拟主机

```
$ cd /etc/nginx/
$ vim conf.d/taiga.conf
```

```
server {
    listen 80;
    server_name taiga.hakase-labs.co;

    large_client_header_buffers 4 32k;
    client_max_body_size 50M;
    charset utf-8;

    access_log /var/log/nginx/taiga.access.log;
    error_log /var/log/nginx/taiga.error.log;

    # Frontend
    location / {
        root /home/taiga/taiga-front-dist/dist/;
        try_files $uri $uri/ /index.html;
    }

    # Backend
    location /api {
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Scheme $scheme;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://127.0.0.1:8001/api;
        proxy_redirect off;
    }

    # Django admin access (/admin/)
    location /admin {
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Scheme $scheme;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://127.0.0.1:8001$request_uri;
        proxy_redirect off;
    }

    # Static files
    location /static {
        alias /home/taiga/taiga-back/static;
    }

    # Media files
    location /media {
        alias /home/taiga/taiga-back/media;
    }

    # Taiga-events
    location /events {
    proxy_pass http://127.0.0.1:8888/events;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_connect_timeout 7d;
    proxy_send_timeout 7d;
    proxy_read_timeout 7d;
    }
}
```

```
$ nginx -t
$ systemctl restart nginx
```

### 允许Nginx访问Taiga前端

```
# 检查Nginx 是否能访问前端文件
$ sudo -u nginx stat /home/taiga/taiga-front-dist/dist

# 将'nginx'用户添加到'taiga'组
$ sudo gpasswd -a nginx taiga

# 授予Web服务器权限以读取taiga-front-dist目录中的所有内容。
$ sudo chmod g+x /home/taiga
$ sudo chmod g+x /home/taiga/taiga-front-dist
$ sudo chmod g+x /home/taiga/taiga-front-dist/dist
```

## Docker

### Docker 简介

> Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从Apache2.0协议开源。  
> Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。  
> 容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。  
> Docker 从 17.03 版本之后分为 CE（Community Edition: 社区版） 和 EE（Enterprise Edition: 企业版）。

### Docker 架构

> Docker 使用客户端-服务器 (C/S) 架构模式，使用远程API来管理和创建Docker容器。  
> Docker 容器通过 Docker 镜像来创建。  
> 容器与镜像的关系类似于面向对象编程中的对象与类。  

Docker |面向对象 
--- |--- 
容器 |对象 
镜像 |类 

![Docker Architecture](./static/image/Sep/docker-arch.png "Docker-Architecture")

Docker 镜像(Images) |Docker 镜像是用于创建 Docker 容器的模板。 
--- |---
Docker 容器(Container) |容器是独立运行的一个或一组应用。 
Docker 客户端(Client) |Docker 客户端通过命令行或者其他工具使用 Docker API (https://docs.docker.com/reference/api/docker_remote_api) 与 Docker 的守护进程通信。 
Docker 主机(Host) |一个物理或者虚拟的机器用于执行 Docker 守护进程和容器。 
Docker 仓库(Registry) |Docker 仓库用来保存镜像，可以理解为代码控制中的代码仓库。<br>Docker Hub(https://hub.docker.com) 提供了庞大的镜像集合供使用。 
Docker Machine |Docker Machine是一个简化Docker安装的命令行工具，通过一个简单的命令行即可在相应的平台上安装Docker，比如VirtualBox、 Digital Ocean、Microsoft Azure。 

### Docker 安装

``` Docker install on CentOS
# Remove old version
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
# Install toolkits
$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
$ Add software repo source
$ sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# Update repo source
$ sudo yum makecache fast
# Install docker ce
$ sudo yum -y install docker-ce
# Start docker
$ sudo systemctl start docker
```

``` Install by scripts
# Download scripts and install by scripts
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
# Start docker
$ sudo systemctl start docker
# Remove docker
$ sudo yum remove docker-ce
$ sudo rm -rf /var/lib/docker
```

> **镜像加速**
> 鉴于国内网络问题，后续拉取 Docker 镜像十分缓慢，我们可以需要配置加速器来解决，我使用的是网易的镜像地址：http://hub-mirror.c.163.com。  
> 新版的 Docker 使用 /etc/docker/daemon.json（Linux） 或者 %programdata%\docker\config\daemon.json（Windows） 来配置 Daemon。  
> 请在该配置文件中加入（没有该文件的话，请先建一个）：  
```
{
  "registry-mirrors": ["http://hub-mirror.c.163.com"]
}
```

### Docker 命令

1. ***docker run*** 命令来在容器内运行一个应用程序。

```
$
```

2. ***docker ps*** 命令来列出容器

3. ***docker stop*** 命令来停止容器

4. ***docker log*** 命令来查看容器内的标准输出

5. ***docker pull*** 命令来从镜像仓库中拉取或者更新指定镜像

6. ***docker port*** 命令来列出指定的容器的端口映射

7. ***docker top*** 命令来查看容器中运行的进程信息

8. ***docker inspect***  命令来获取容器/镜像的元数据

9. ***docker images*** 命令来列出本地主机上的镜像

10. ***docker search*** 命令来搜索镜像

11. ***docker build*** 命令用于使用 Dockerfile 创建镜像

12. ***docker tag*** : 标记本地镜像，将其归入某一仓库

13. ***docker info*** : 显示 Docker 系统信息，包括镜像和容器数



> 当运行容器时，使用的镜像如果在本地中不存在，docker 就会自动从 docker 镜像仓库中下载，默认是从 Docker Hub 公共镜像源下载。

### Error Process

> Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.40/images/search?limit=25&term=nginx: dial unix /var/run/docker.sock: connect: permission denied

```
# 将当前用户加入docker组:
$ sudo gpasswd -a ${USER} docker
# 退出当前用户
$ su root
# 切换回当前用户
$ su admin
```

> docker是1.12或以上的版本，可以修改（或新建）daemon.json文件。修改后会立即生效，不需重启docker服务。
> vim /etc/docker/daemon.json  
> { 
>   "registry-mirrors":  
>   [  
>     "https://registry.docker-cn.com",  
>     "https://docker.mirrors.ustc.edu.cn"  
>   ],  
>   "debug": true,  
>   "log-level": "info",  
>   "graph": "/home/root/docker-data"  
> }  

### Window(Windows Server 2016) 安装 docker

```
# 以管理员权限运行PowerShell
Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force #指定Nuget安装包最小版本
Install-Module -Name DockerMsftProvider -Force                #安装docker模块
Install-Package -Name docker -ProviderName DockerMsftProvider -Force #安装docker包
Restart-Computer -Force #重新启动
```

```
Install-Module -Name DockerMsftProvider -Repository PSGallery -Force
Install-Package -Name docker -ProviderName DockerMsftProvider
Restart-Computer -Force
```

#### Error Process

```
Find-Package –providerName DockerMsftProvider | Install-Package -Verbose
```