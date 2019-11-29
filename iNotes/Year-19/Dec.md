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
$ docker run -p 7070:80 --name nginx -d nginx
$ docker cp nginx:/etc/nginx/nginx.conf /home/git/nginx/nginx.conf
$ docker cp nginx:/etc/nginx/conf.d /home/git/nginx/conf.d
$ # docker run --privileged=true --restart=always -p 7070:80 \
    --name=nginx --network=nginx-net \
    --network-alias=nginx \
    -v /home/git/nginx/www:/www \
    -v /home/git/nginx/nginx.conf:/etc/nginx/nginx.conf \
    -v /home/git/nginx/conf.d:/etc/nginx/conf.d \
    -v /home/git/nginx/logs:/var/log/nginx \
    -v /home/git/nginx/www/logs:/wwwlogs \
    -d nginx
$ # docker run --privileged=true --restart=always -p 7070:80 \
    --name=nginx --network=nginx-net \
    --network-alias=nginx \
    --ip=172.18.0.2 \
    -v /home/git/nginx/www:/www \
    -v /home/git/nginx/nginx.conf:/etc/nginx/nginx.conf \
    -v /home/git/nginx/conf.d:/etc/nginx/conf.d \
    -v /home/git/nginx/logs:/var/log/nginx \
    -v /home/git/nginx/www/logs:/wwwlogs \
    -d nginx
$ docker run --privileged=true --restart=always -p 7070:80 \
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
$ # docker network create gitea-net
$ # docker run --privileged=true --restart=always -d \
    --network=gitea-net --network-alias=gitea \
    --name=gitea -p 1022:22 -p 3000:3000 \
    -v /home/git/gitea:/data gitea/gitea:latest
$ docker run --privileged=true --restart=always -d \
    --network=nginx-net --network-alias=gitea \
    --ip=172.19.0.3 \
    --name=gitea -p 1022:22 -p 3000:3000 \
    -v /home/git/gitea:/data gitea/gitea:latest
```

``` showdoc
$ # docker network create showdoc-net
$ # docker run --privileged=true --restart=always --name=showdoc showdoc -d -p 9090:80 -v /home/git/showdoc/html:/var/www/html/  star7th/showdoc
$ docker run --privileged=true \
    --restart=always \
    --network=nginx-net --network-alias=showdoc \
    --ip=172.19.0.4 \
    --name=showdoc -d -p 9090:80 \
    -v /home/git/showdoc/html:/var/www/html/  star7th/showdoc
```

``` wordpress
$ docker network create wp-net --subnet=172.20.0.0/16

$ docker run --name=wp-mysql --privileged=true --restart=always \
    --network wp-net --network-alias mysql \
    --ip=172.20.0.2 \
    -v /home/git/wordpress/mysql:/var/lib/mysql -v /home/git/wordpress/mysql/conf:/etc/mysql/conf.d \
    -e MYSQL_ROOT_PASSWORD=avatech@2019 -p 7706:3306 -d mysql:5.6

$ docker run --name wp-web --privileged=true --restart=always \
    --network wp-net --network-alias wordpress \
    --ip=172.20.0.3 \
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
$ # docker run --privileged=true \
    --restart=always \
    --ip=172.17.0.3 \
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

*docker-compose.yml*

```
docker run \
  --link gitea:gitea \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --volume=/home/admin/drone:/data \
  --env=DRONE_GITEA_SERVER=http://gitea:3000/ \  #这里配置的是Gitea对应Docker的访问地址, 如果使用必须link, 也可以是192.168.3.15:3000这种
  --env=DRONE_GIT_ALWAYS_AUTH=false \
  --env=DRONE_RUNNER_CAPACITY=2 \
  --env=DRONE_SERVER_HOST=192.168.3.15:9090 \   #这里主机地址为drone安装后的访问地址, 
  --env=DRONE_SERVER_PROTO=http \
  --env=DRONE_TLS_AUTOCERT=false \
  --publish=9090:80 \
  --restart=always \
  --detach=true \
  --name=drone \
  drone/drone:latest
docker run \
  --link gitea:gitea \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --volume=/home/admin/drone:/data \
  --env=DRONE_AGENTS_ENABLED=true \
  --env=DRONE_GITEA_SERVER=http://gitea:3000/ \
  --env=DRONE_GITEA_CLIENT_ID=4e5d2dee-0533-48ad-9e03-70af6e7bb8ed \
  --env=DRONE_GITEA_CLIENT_SECRET=EnA9acNIS0sLyeKYeMTco0RvE3eAmsksR-Ne4v-HxMY= \
  --env=DRONE_SERVER_HOST=192.168.3.15:9090 \
  --env=DRONE_SERVER_PROTO=http \
  --publish=9090:80 \
  --publish=1443:443 \
  --restart=always \
  --detach=true \
  --name=drone \
  drone/drone:latest

docker run \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --volume=/home/admin/drone:/data \
  --env=DRONE_GITEA_SERVER=http://192.168.3.15:3000/ \
  --env=DRONE_GIT_ALWAYS_AUTH=false \
  --env=DRONE_RUNNER_CAPACITY=2 \
  --env=DRONE_SERVER_HOST=192.168.5.15:9090 \
  --env=DRONE_SERVER_PROTO=http \
  --env=DRONE_TLS_AUTOCERT=false \
  --publish=9090:80 \
  --restart=always \
  --detach=true \
  --name=drone \
  drone/drone:latest


version: '2'

services:
  drone-server:
  image: drone/drone:latest
  ports: - 9090:80
  volumes: - /home/admin/drone:/data
  restart: always
  environment: - DRONE_OPEN=true - DOCKER_API_VERSION=1.24 - DRONE_HOST=10.1.86.206 - DRONE_GOGS=true - DRONE_GITEA_URL=http://192.168.3.15:3000/ - DRONE_SECRET=ok

  drone-agent:
  image: drone/drone:latest
  command: agent
  restart: always
  depends_on: - drone-server
  volumes: - /var/run/docker.sock:/var/run/docker.sock
  environment: - DOCKER_API_VERSION=1.24 - DRONE_SERVER=ws://drone-server:8000/ws/broker - DRONE_SECRET=ok

docker run \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --volume=/home/admin/drone:/data \
  --env=DRONE_AGENTS_ENABLED=true \
  --env=DRONE_GITEA_SERVER=http://192.168.3.15:3000 \
  --env=DRONE_GITEA_CLIENT_ID=f670c650-1430-41b9-8792-8f83dfa2ff63 \
  --env=DRONE_GITEA_CLIENT_SECRET=dUJJOJOT8vvLa0tcmijySoEyorkcGcCFV-ggc81wgWg= \
  --env=DRONE_GITEA_PRIVATE_MODE=false \
  --env=DRONE_SERVER_HOST=192.168.3.15:9090 \
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
  --env=DRONE_GITEA_SERVER=http://192.168.3.15:3000 \
  --env=DRONE_GIT_ALWAYS_AUTH=false \
  --env=DRONE_GITEA_PRIVATE_MODE=false \
  --env=DRONE_GITEA_SKIP_VERIFY=true \
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


# right
docker run \
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

docker run \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --volume=/home/admin/drone:/data \
  --env=DRONE_AGENTS_ENABLED=true \
  --env=DRONE_GITEA=true \
  --env=DRONE_GITEA_SKIP_VERIFY=true \
  --env=DRONE_GITEA_SERVER=http://182.92.165.53:7070/gitea/ \
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


docker run \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --volume=/home/admin/drone:/data \
  --env=DRONE_AGENTS_ENABLED=true \
  --env=DRONE_GITEA_SERVER=http://182.92.165.53:7070/gitea/ \
  --env=DRONE_GITEA_CLIENT_ID=cc17db57-3c5d-42fe-ae66-b621e0c0ea7f \
  --env=DRONE_GITEA_CLIENT_SECRET=phIrUusTg56eFwSN33MldDABC68-Fv2t7rGBXJFK2io= \
  --env=DRONE_SERVER_HOST=192.168.3.15:9090 \
  --env=DRONE_USER_CREATE=username:administrator,admin:true \
  --env=DRONE_SERVER_PROTO=http \
  --publish=9090:80 \
  --publish=443:443 \
  --restart=always \
  --detach=true \
  --name=drone \
  drone/drone:latest

c88642882aceaab7eb69db88a5f80836f1d45ba4
```