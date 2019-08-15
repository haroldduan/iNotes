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