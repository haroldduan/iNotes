<!--
@author: harold.duan
@date: 19-12-01
@memo: Notes logging
-->

# ShowDoc

## ShowDoc install in docker

```
$ mkdir showdoc showdoc/html
$ chmod -R 777 showdoc/
# $ docker pull star7th/showdoc:v2.4.17
$ docker pull star7th/showdoc
$ docker network create showdoc-net
$ docker run --restart=always --privileged=true --name=showdoc --network showdoc-net --network-alias showdoc -d -p 9090:80 -v /home/git/showdoc/html:/var/www/html/  star7th/showdoc
```

**Error process**

+ ***docker: Error response from daemon: driver failed programming external connectivity on endpoint showdoc (0c46ef5d9fea4dfe91278992cf814ea34d3cf96f4d539b959c2a5d53893f15ae):  (iptables failed: iptables --wait -t nat -A DOCKER -p tcp -d 0/0 --dport 9090 -j DNAT --to-destination 172.17.0.4:80 ! -i docker0: iptables: No chain/target/match by that name.***

*solution:* ***Restart docker***

```
$ sudo service docker restart
```


# Https

## OpenSSL

### install OpenSSL

```
$ sudo yum install -y openssl openssl-devel
```


```
# generating private key
$ sudo openssl genrsa -des3 -out server.key 1024
# generating CSR
$ sudo openssl req -new -key server.key -out server.csr -config openssl.cn
# generating CA sign
$ sudo openssl req -new -x509 -keyout ca.key -out ca.crt -config openssl.cnf
# CA CSR => CRT
$ sudo openssl ca -in server.csr -out server.crt -cert ca.crt -keyfile ca.key -config openssl.cnf
# CRT Key => pem
$ sudo cat server.crt server.key > server.pem
```

### Error processing

+ CSR => CRT

**/etc/pki/CA/index.txt: No such file or directory**  
**unable to open '/etc/pki/CA/index.txt'**

*solution*

```
$ sudo vim /etc/pki/tls/openssl.cnf 
> dir             = ./CA  
$ sudo mkdir -p CA/newcerts
$ sudo touch CA/index.txt
$ sudo touch CA/serial
$ sudo echo "01" > CA/serial

$ sudo openssl ca -in server.csr -out server.crt -cert ca.crt -keyfile ca.key -config openssl.cnf
```
+ **nginx: [emerg] SSL_CTX_use_PrivateKey_file("/etc/nginx/ssl/server.key") failed (SSL: error:0906406...PEM lib)**

*solution*

```
$ openssl rsa -in server.key -out unserver.key  
$ cp ./unserver.key server.key
$ rm -rf unserver.key
$ service nginx restart
```

## Nginx

### Install Nginx

+ Add repo

*默认情况Centos7中无Nginx的源*

```
$ sudo rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
```

+ Install nginx

```
$ sudo yum install -y nginx
```

+ Configuration

*开机启动*

```
$ sudo systemctl start nginx.service
$ sudo systemctl enable nginx.service
```

**Error processing**

+ *2019/11/25 17:42:27 [crit] 20020#20020: *1 connect() to [::1]:3000 failed (13: Permission denied) while connecting to upstream, client: 192.168.3.208, server: rds.avatech, request: "GET /gitea/ HTTP/1.1", upstream: "http://[::1]:3000/", host: "192.168.3.15"*


### Install Nginx on docker

```
$ docker install nginx
```

```
$ docker network create nginx-net --subnet=172.19.0.0/16
$ docker run -p 8080:80 --name nginx -d nginx
$ docker cp nginx:/etc/nginx/nginx.conf /home/git/nginx/nginx.conf
$ docker cp nginx:/etc/nginx/conf.d /home/git/nginx/conf.d
$ docker run --privileged=true --restart=always -p 8080:80 \
    --name=nginx \
    --network=nginx-net \
    --network-alias=nginx \
    --ip=172.19.0.2 \
    -v /home/git/nginx/www:/usr/share/nginx/html \
    -v /home/git/nginx/nginx.conf:/etc/nginx/nginx.conf \
    -v /home/git/nginx/conf.d:/etc/nginx/conf.d \
    -v /home/git/nginx/logs:/var/log/nginx \
    -d nginx
```

``` php
$ docker pull php:rc-fpm
$ docker run -p 9000:9000 \
    --network=nginx-net \
    --network-alias=phpfpm \
    --ip=172.19.0.5 \
    --name  phpfpm -v /home/git/nginx/www:/var/www/html \
    -v /home/git/nginx/www/conf:/usr/local/etc/php \
    -v /home/git/nginx/www/logs:/phplogs \
    -d php:rc-fpm
$ docker run \
    --network=nginx-net \
    --network-alias=phpfpm \
    --ip=172.19.0.5 \
    -d \
    -p 9000:9000 \
    -v /home/git/nginx/www:/usr/share/nginx/html \
    --name phpfpm bitnami/php-fpm:7.0
```

``` gitea
$ docker network create gitea-net --subnet=172.20.0.0/16
$ # docker run --privileged=true --restart=always -d \
    --network=gitea-net --network-alias=gitea \
    --ip=172.20.0.2 \
    --name=gitea -p 1022:22 -p 3000:3000 \
    -v /home/git/gitea:/data gitea/gitea:latest
$ docker run --privileged=true --restart=always -d \
    --network=gitea-net --network-alias=gitea \
    --ip=172.20.0.2 \
    --name=gitea -p 1022:22 -p 7070:3000 \
    -v /home/git/gitea:/data gitea/gitea:1.10.0
```

``` showdoc
$ docker network create showdoc-net --subnet=172.21.0.0/16
$ docker run --privileged=true \
    --restart=always \
    --network=showdoc-net --network-alias=showdoc \
    --ip=172.21.0.2 \
    --name=showdoc -d -p 6060:80 \
    -v /home/git/showdoc/html:/var/www/html/  star7th/showdoc
```

``` drone
$ docker pull drone/drone:latest
$ docker network create drone-net --subnet=172.22.0.0/16
$ docker run \
    --privileged=true --restart=always -d \
    --network=drone-net --network-alias=drone \
    --ip=172.22.0.2 \
    --volume=/var/run/docker.sock:/var/run/docker.sock \
    --volume=/home/git/drone:/data \
    --env=DRONE_AGENTS_ENABLED=true \
    --env=DRONE_GITEA_SERVER=http://rds.avatech.com.cn:7070/ \
    --env=DRONE_GITEA_CLIENT_ID=d12d3a96-bdb5-45f4-b0e1-fd30ad6d3282 \
    --env=DRONE_GITEA_CLIENT_SECRET=Sc5gMjqVUZTg75ISR7Ak7dW9rfeKYhG4JcyZC_Jw_ic= \
    --env=DRONE_SERVER_HOST=http://rds.avatech.com.cn:5050/ \
    --env=DRONE_USER_CREATE=username:codebot,admin:false \
    --env=DRONE_SERVER_PROTO=http \
    --publish=5050:80 \
    --publish=443:443 \
    --restart=always \
    --detach=true \
    --name=drone \
    drone/drone:latest
```

``` wordpress
$ docker network create wp-net --subnet=172.23.0.0/16
$ docker run --name=wp-mysql --privileged=true --restart=always \
    --network wp-net --network-alias mysql \
    --ip=172.23.0.2 \
    -v /home/git/wordpress/mysql:/var/lib/mysql -v /home/git/wordpress/mysql/conf:/etc/mysql/conf.d \
    -e MYSQL_ROOT_PASSWORD=avatech@2019 -p 7706:3306 -d mysql:5.6

$ docker run --name wp-web --privileged=true --restart=always \
    --network wp-net --network-alias wordpress \
    --ip=172.23.0.3 \
    --link wp-mysql:mysql \
    -e WORDPRESS_DB_PASSWORD=avatech@2019 -p 8000:80 \
    -v /home/git/wordpress/wordpress-html:/var/www/html -d wordpress:latest
```

``` portainer
$ docker network create portainer-net --subnet=172.18.0.0/16
$ docker run --privileged=true \
    --restart=always \
    --network=portainer-net --network-alias=portainer \
    --ip=172.18.0.2 \
    -p 9000:9000 \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /home/git/portainer:/data \
    --name portainer \
    -d docker.io/portainer/portainer
```


upstream portainer {
    server localhost:9000;
    keepalive 2000;
}

upstream gitea {
    server 172.17.0.4:3000;
    keepalive 2000;
}

upstream gitea {
    server localhost:3000;
    keepalive 2000;
}
server {
    listen 80;
    server_name    rds.avatech;
    # listen       443 ssl;
    # server_name  rds.avatech;
    # ssl_certificate      /etc/nginx/ssl/server.crt;
    # ssl_certificate_key  /etc/nginx/ssl/server.key;
    # ssl_session_timeout  5m;
    # ssl_protocols  SSLv2 SSLv3 TLSv1;
    # ssl_ciphers  HIGH:!aNULL:!MD5;
    # ssl_prefer_server_ciphers   on;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
    location /gitea/ {
        proxy_pass http://gitea/;
        proxy_set_header Host $host:$server_port;
    }
}

# Drone

## Install Drone in docker

```

```

```
# gogs
$ docker run \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --volume=/home/admin/drone:/data \
  --env=DRONE_AGENTS_ENABLED=true \
  --env=DRONE_GOGS=true \
  --env=DRONE_GOGS_SKIP_VERIFY=true \
  --env=DRONE_GOGS_SERVER=http://182.92.165.53:8080 \
  --env=DRONE_RUNNER_CAPACITY=2 \
  --env=DRONE_SERVER_HOST=192.168.3.15:9090 \
  --env=DRONE_SERVER_PROTO=http \
  --env=DRONE_USER_CREATE=username:administrator,admin:true \
  --publish=9090:80 \
  --publish=443:443 \
  --restart=always \
  --detach=true \
  --name=drone \
  drone/drone:latest

# gitea
$ docker run \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --volume=/home/admin/drone:/data \
  --env=DRONE_AGENTS_ENABLED=true \
  --env=DRONE_GITEA_SERVER=http://182.92.165.53:7070/ \
  --env=DRONE_GITEA_CLIENT_ID=db84ac14-0f36-4240-be85-c4e63a70face \
  --env=DRONE_GITEA_CLIENT_SECRET=jwdWwo2XgKKJ3oJsQuafzM3GccyQ2p-MR0_OoNpQ1tE= \
  --env=DRONE_SERVER_HOST=192.168.3.15:9090 \
  --env=DRONE_USER_CREATE=username:administrator,admin:true \
  --env=DRONE_SERVER_PROTO=http \
  --publish=9090:80 \
  --publish=443:443 \
  --restart=always \
  --detach=true \
  --name=drone \
  drone/drone:latest


docker run \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --volume=/home/admin/drone:/data \
  --env=DRONE_AGENTS_ENABLED=true \
  --env=DRONE_GITEA_SERVER=http://182.92.165.53:7070/ \
  --env=DRONE_GITEA_CLIENT_ID=1497497f-1555-42fa-a597-6e37f083ae64 \
  --env=DRONE_GITEA_CLIENT_SECRET=kT649PjMIaRdsvzkEgI9wbY5X259uPJpA9xluxuNm0g= \
  --env=DRONE_SERVER_HOST=192.168.3.15:9090 \
  --env=DRONE_USER_CREATE=username:administrator,admin:true \
  --env=DRONE_SERVER_PROTO=http \
  --publish=9090:80 \
  --publish=443:443 \
  --restart=always \
  --detach=true \
  --name=drone \
  drone/drone:latest

docker run \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --volume=/home/admin/drone:/data \
  --env=DRONE_AGENTS_ENABLED=true \
  --env=DRONE_GITEA_SERVER=http://182.92.165.53:7070/ \
  --env=DRONE_GITEA_CLIENT_ID=755a67f5-e0a2-4824-b348-cd3638d0aa1b \
  --env=DRONE_GITEA_CLIENT_SECRET=eKoKj-LdCuihwIRqAL1ZxbtBGHHz16iGZBA4JOVW5P4= \
  --env=DRONE_SERVER_HOST=192.168.3.15:9090 \
  --env=DRONE_USER_CREATE=username:codebot,admin:true \
  --env=DRONE_SERVER_PROTO=http \
  --publish=9090:80 \
  --publish=443:443 \
  --restart=always \
  --detach=true \
  --name=drone \
  drone/drone:latest

docker run \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --volume=/home/git/drone:/data \
  --env=DRONE_AGENTS_ENABLED=true \
  --env=DRONE_GITEA_SERVER=http://182.92.165.53:7070/ \
  --env=DRONE_GITEA_CLIENT_ID=f7751024-380c-47cd-acae-234a1d40f508 \
  --env=DRONE_GITEA_CLIENT_SECRET=Jk3WfsQ1D9YPkwJMjdI2qdaTfwC2yeCpLOAUOcfBOXc= \
  --env=DRONE_SERVER_HOST=182.92.165.53:5050 \
  --env=DRONE_USER_CREATE=username:codebot,admin:true \
  --env=DRONE_SERVER_PROTO=http \
  --publish=5050:80 \
  --publish=443:443 \
  --restart=always \
  --detach=true \
  --name=drone \
  drone/drone:latest
```

```
$ ls -lhk
$ ls -lahk
$ tar -cvf test.tar test
$ tar -zcvf test.tar.gz test
$ tar -zxvf /usr/local/test.tar.gz
```

``` MediaDrop
$ docker pull acaranta/mediadrop
$ docker run --name mediadrop -p 8080:8080 -d acaranta/mediadrop # http://192.168.3.15:8080/admin admin/admin

# MySQL /var/lib/mysql/ /etc/mysql/
# MediaDrop /mediadrop /mediadrop/development.ini
# mysql -u username -h localhost -p
# pass
# show tables;
# select * from users;
```

``` Razuna
$ docker pull netresearch/razuna
$ docker run --name razuna -p 7070:8080 -d netresearch/razuna
```

```
$ git clone https://github.com/skaro13/docker-mediadrop.git
$ docker build \
  --no-cache \
  --pull \
  -t skaro13/mediadrop:latest .
$ docker pull skaro13/mediadrop
$ docker pull mysql:5.7
$ docker network create mediadrop-net --subnet=172.24.0.0/16
$ docker run --name=mediadrop-mysql --privileged=true --restart=always \
    --network mediadrop-net --network-alias mysql \
    --ip=172.24.0.2 \
    -v /home/git/mediadrop/mysql/data:/var/lib/mysql \
    -v /home/git/mediadrop/mysql/conf:/etc/mysql/conf.d \
    -e MYSQL_ROOT_PASSWORD=avatech@2019 \
    -e MYSQL_DATABASE=mediadrop \
    -e MYSQL_USER=mediadrop \
    -e MYSQL_PASSWORD=avatech@2019 \
    -d mysql:5.7
# docker create
$ docker run --name=mediadrop-web --privileged=true --restart=always \
  --network mediadrop-net --network-alias mediadrop \
  --ip=172.24.0.3 \
  -e DB_HOST=mediadrop-mysql \
  -e DB_NAME=mediadrop \
  -e DB_USER=mediadrop \
  -e DB_PASSWORD=avatech@2019 \
  -p 9090:8080 \
  -v /home/git/mediadrop/mediadrop/data:/data \
  -d skaro13/mediadrop
```

```
version: '2'
services:
  mediadrop:
    container_name: md-web
    image: skaro13/mediadrop
    environment:
      - DB_HOST=md-mysql
      - DB_NAME=mediadrop
      - DB_USER=mediadrop
      - DB_PASSWORD=avatech@2019
    volumes:
      - /home/git/mediadrop/mediadrop/data:/data
    ports:
      - "9090:8080"
    depends_on:
      - db
    restart: always
  db:
    container_name: md-mysql
    image: mysql:5.7
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=avatech@2019
      - MYSQL_DATABASE=mediadrop
      - MYSQL_USER=mediadrop
      - MYSQL_PASSWORD=avatech@2019
    volumes:
      - /home/git/mediadrop/mysql/data:/var/lib/mysql
      - /home/git/mediadrop/mysql/conf:/etc/mysql/conf.d
```

docker run --privileged=true \
    --restart=always \
    --name=showdoc -d -p 6060:80 \
    -v /home/admin/showdoc/html:/var/www/html/  star7th/showdoc