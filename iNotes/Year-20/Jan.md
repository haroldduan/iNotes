<!--
@author: harold.duan
@date: 20-01-01
@memo: Notes logging
-->

## User configuration

```
$ useradd admin
$ passwd admin
$ chmod u+w /etc/sudoers
$ vim /etc/sudoers
# admin   ALL=(ALL)       ALL
% chmod u-w /etc/sudoers
```

```
$ sudo service firewalld start
$ sudo firewall-cmd --zone=public --add-port=22/tcp --permanent
$ sudo firewall-cmd --reload
$ sudo firewall-cmd --list-all
```

## Softwares Install

+ Glances install

```
$ yum install epel* -y
$ yum install python-pip python-devel -y
$ yum install glances -y
```

+ Git install

```
$ sudo yum install git -y
```

+ Docker install

```
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
# $ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
$ sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
$ sudo yum install docker-ce docker-ce-cli containerd.io
```

```
$ sudo service docker start
$ sudo systemctl enable docker
```

```
$ sudo groupadd docker
$ sudo gpasswd -a $USER docker
$ newgrp docker

$ sudo vim /etc/docker/daemon.json
# input this contents
{
  "registry-mirrors":
  [
    "https://registry.docker-cn.com",
    "https://docker.mirrors.ustc.edu.cn",
    "http://hub-mirror.c.163.com"
  ],
  "debug": true,
  "log-level": "info",
  "graph": "/home/admin/docker-data"
}

$ sudo service docker restart
```

+ Python3 install

```
$ sudo yum install python3
```

+ Docker-Compose install

```
$ sudo pip3 install docker-compose
```

+ Copying the public key to server

```
$ scp id_rsa.pub admin@182.92.239.69:/tmp
$ cd /tmp

$ chmod 700 ~/.ssh
$ cat id_rsa.pub >> ~/.ssh/authorized_keys
$ chmod 600 ~/.ssh/authorized_keys

$ sudo vim /etc/ssh/sshd_config
$ RSAAuthentication yes
$ PubkeyAuthentication yes
$ AuthenticationMethods publickey,password

$ sudo service sshd restart
```

+ Portainer install

```
$ docker pull docker.io/portainer/portainer
$ docker run -p 9000:9000 \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /home/admin/portainer:/data \
    --name portainer \
    -d docker.io/portainer/portainer
```

```
$ sudo yum history list maven
$ sudo yum history undo ID
```