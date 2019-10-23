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
# $ docker run -d --privileged=true --name wp-mysql -v /data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=avatech@2019 -p 7706:3306 mysql:latest
# $ docker run -d --privileged=true --name wp-mysql -v /data/mysql:/var/lib/mysql -v /data/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=avatech@2019 -p 7706:3306 -d mysql:latest
# $ docker run -d --privileged=true --name wp-mysql --network wp-net --network-alias mysql -v /data/mysql:/var/lib/mysql -v /data/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=avatech@2019 -p 7706:3306 -d mysql:latest
$ docker run -d --privileged=true --name wp-mysql --network wp-net --network-alias mysql -v /data/mysql:/var/lib/mysql -v /data/mysql/conf:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=avatech@2019 -p 7706:3306 -d mysql:5.6
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
# $ docker run -d --name wp-web -e WORDPRESS_DB_HOST=mysql -e WORDPRESS_DB_PASSWORD=avatech@2019 -p 1080:80 --link wp-mysql:mysql -v /data/wordpress-html:/var/www/html wordpress:latest
$ docker run -d --name wp-web --network wp-net --network-alias wordpress -e WORDPRESS_DB_HOST=mysql -e WORDPRESS_DB_PASSWORD=avatech@2019 -p 1080:80 --link wp-mysql:mysql -v /data/wordpress-html:/var/www/html wordpress:latest
$ docker run -d --name wp-web --network wp-net --network-alias wordpress -e WORDPRESS_DB_PASSWORD=avatech@2019 -p 1080:80 -v /data/wordpress-html:/var/www/html wordpress:latest
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
$ docker run --privileged=true --name gitea-postgres --network gitea-net --network-alias postgres -e POSTGRES_PASSWORD=avatech@2019 -p 5432:5432 -v /data/pgsql:/var/lib/pgsql -d postgres:9.6
```

## gitea docker install

``` command
$ docker pull gitea/gitea:latest
```

## gitea docker running

``` command
# $ docker run -d --name=gitea -p 1022:22 -p 3000:3000 -v /home/admin/gitea:/data gitea/gitea:latest
# $ docker run -d --name=gitea --network wp-net --network-alias gitea -p 1022:22 -p 3000:3000 --link wp-mysql:mysql -v /home/admin/gitea:/data gitea/gitea:latest
# $ docker run -d --name=gitea --network gitea-net --network-alias gitea -p 1022:22 -p 3000:3000 --link gitea-postgres:postgres -v /home/admin/gitea:/data gitea/gitea:latest
$ docker run -d --name=gitea -p 1022:22 -p 3000:3000 -v /home/admin/gitea:/data gitea/gitea:latest
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