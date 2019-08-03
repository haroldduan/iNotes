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