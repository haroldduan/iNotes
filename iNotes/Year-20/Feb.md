<!--
@author: harold.duan
@date: 20-02-01
@memo: Notes logging
-->

## Webmin

### Install Webmin

```
$ sudo vi /etc/yum.repos.d/webmin.repo

# /etc/yum.repos.d/webmin.repo
[Webmin]
name=Webmin Distribution Neutral
#baseurl=http://download.webmin.com/download/yum
mirrorlist=http://download.webmin.com/download/yum/mirrorlist
enabled=1

$ wget http://www.webmin.com/jcameron-key.asc
$ sudo rpm --import jcameron-key.asc

$ sudo yum install webmin

# set your system root's password
# sudo passwd root
```

### Install Webmin in Docker

```
# $ docker pull tragus/webmin
# $ docker run -p 80:80 -v /home/admin/webmin/var:/var/webmin -v /home/admin/webmin/etc:/etc/webmin --name webmin -v /var/run/webmin:/var/run/container-control -d tragus/webmin
# $ docker run -p 80:80 -v /home/admin/webmin/var:/var/webmin -v /home/admin/webmin/etc:/etc/webmin --name webmin -d tragus/webmin

$ docker pull piersonjarvis/webmin-samba
# $ docker run -d -p 80:9090 -p 139:139 -p 445:445 -v /webmin:/media/storage piersonjarvis/webmin-samba
$ docker run -d -p 80:80 -p 139:139 -p 445:445 -v /home/admin/webmin:/media/storage --name=webmin piersonjarvis/webmin-samba
$ docker run -d -p 80:80 -p 139:139 -p 445:445 -v /home/admin/webmin/media/storage:/media/storage -v /home/admin/webmin/var:/var/webmin -v /home/admin/webmin/etc:/etc/webmin --name=webmin piersonjarvis/webmin-samba
$ docker run -d -p 8000:80 -p 139:139 -p 445:445 -v /home/admin/webmin/media/storage:/media/storage -v /home/admin/webmin/var:/var/webmin -v /home/admin/webmin/etc:/etc/webmin --name=webmin piersonjarvis/webmin-samba
```

## Cockpit

### Install Cockpit

```
$ sudo yum install cockpit
$ sudo systemctl enable --now cockpit.socket
$ sudo firewall-cmd --add-service=cockpit
# sudo firewall-cmd --add-service=cockpit --permanent
```

## SFTP Configuration

```
$ su root
$ groupadd sftp
$ mkdir /data/sftp
# -g 用户组； -d 指定家目录； -s 不登陆； -M 不创建家目录 
$ useradd -g sftp -d /data/sftp/harold -s /sbin/nologin harold
$ echo avatech@2020 | passwd --stdin harold
```

```
$ vim /etc/ssh/sshd_config

#Subsystem      sftp    /usr/libexec/openssh/sftp-server
Subsystem sftp internal-sftp
Match Group sftp
ChrootDirectory /data/sftp/%u
ForceCommand internal-sftp

# 下面两项是与安全有关
AllowTcpForwarding no
X11Forwarding no
```

```
$ chown root:root /data/sftp/harold
$ chmod 755 /data/sftp/harold
```

```
$ mkdir -p /data/sftp/harold/workspace
$ chown harold:sftp /data/sftp/harold/workspace
$ chmod 755 /data/sftp/harold/workspace
```

**Error Process**

*Permission denied (publickey).*
*Couldn't read packet: Connection reset by peer.*

```
$ chmod 0600 ~/.ssh/id_rsa
```
