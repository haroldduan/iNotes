<!--
@author: harold.duan
@date: 19-07-01
@memo: Notes logging
-->

# git command memo

``` git command
# 查看本地远程分支的跟踪情况
git remote show origin

# 同步删除本地远程分支
git remote prune
```

# install openjdk && openjrk

``` command
# install openjdk
$ sudo apt install openjdk-11-jdk

# install openjre
$ sudo apt install openjdk-11-jre
```

# install maven

``` command
$ sudo apt install maven
```

# install vscode plugin for java,maven,springboot

# yum 删除软件及其依赖

```
$ sudo yum history list maven

$ sudo yum history undo ID
```

# maven manual installation 

```
# 1 Prerequsities,install java jdk
# 2 Download Apache Maven 3.6.0
$ cd /opt
$ wget https://www-eu.apache.org/dist/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz
# 3 Extract archive,create Symbolic Link
$ sudo tar xzf apache-maven-3.6.0-bin.tar.gz
$ sudo ln -s apache-maven-3.6.0 maven
# 4 Setup environment variables
$ sudo vim /etc/profile.d/maven.sh
# Input this contents
export M2_HOME=/opt/maven
export PATH=${M2_HOME}/bin:${PATH}
$ source /etc/profile.d/maven.sh
```

# jupyter install

## jupyter install

```
# Upgrade pip(3)
$ python(3) -m pip install --upgrade pip --user
$ python(3) -m pip install jupyter --user
# $ pip(3) install --upgrade pip --user
# $ pip(3) install jupyter --user
# jupyter --version
```

## matplotlib install

```
$ pip(3) install matplotlib --user
```

## jupyter_contrib_nbextensions install

```
# install nbextensions & nbextensions_configurator
$ pip(3) install jupyter_nbextensions_configurator jupyter_contrib_nbextensions --user
# nbextension list install
$ jupyter contrib nbextension install --user
# nbextension_configurator enable
$ jupyter nbextensions_configurator enable --user
```

# MariaDB install

## Remove old version

```
$ sudo rpm -qa | grep mariadb
$ sudo yum remove mariadb-server-5.5.60-1.el7_5.x86_64
$ sudo yum remove mariadb-5.5.60-1.el7_5.x86_64
$ sudo yum remove mariadb-libs-5.5.60-1.el7_5.x86_64
$ sudo yum remove MariaDB-server-10.4.6-1.el7.centos.x86_64
$ sudo yum remove MariaDB-client-10.4.6-1.el7.centos.x86_64
```

## Create repo

```
$ sudo vim /etc/yum.repos.d/mariadb.repo

[mariadb]
name = MariaDB
baseurl = http://mirrors.aliyun.com/mariadb/yum/10.4/centos7-amd64/
gpgkey =  http://mirrors.aliyun.com/mariadb/yum/RPM-GPG-KEY-MariaDB
gpgcheck = 1
```
## Clean cache & recreate cache

```
$ sudo yum clean all
$ sudo yum makecache
$ sudo yum list --disablerepo=\* --enablerepo=mariadb
```

## Install MariaDB

```
$ sudo yum install MariaDB-client MariaDB-server MariaDB-devel -y
```

## Start MariaDB

```
$ sudo systemctl start mariadb
```

## Set auto start

```
$ sudo systemctl enable mariadb
```

## Error process

1. > ERROR 1698 (28000): Access denied for user 'root'@'localhost'

> 对于 ERROR 1045 (28000): Access denied for user ['root'@'localhost'] 此类错误返回时， (using password: ?)中?的关键字是YES还是NO，关键不在于用户是否存在，密码是否正确，它的结果取决于登录时，用户对于密码有没有字符串的输入，如果没有，MySQL数据库验证后，若出错返回此类信息，则应是 (using password: NO)，若用户对密码有字符串的输入，返回的则是(using password: YES)。

2. > Access denied for user 'admin'@'192.168.3.208' (using password: YES)

```
# Create user
$ create user admin@localhost identified by 'password';
# grant user
$ grant all privileges on *.* to 'admin'@'%' identified by 'avatech@2019' with grant option;
$ flush privileges;
# $ select host,user,password from user;
# $ alter user admin@localhost identified by 'avatech@2019';
# $ sudo mysql -u admin -pavatech@2019
```

3. Important configuration files

```
$ sudo vim /etc/my.cnf
/etc/my.cnf
/etc/my.cnf.d
/etc/my.cnf.d/server.cnf
/etc/my.cnf.d/mysql-clients.cnf
```