<!--
@author: harold.duan
@date: 20-12-01
@memo: Notes logging
-->

### YApi

*[GitHub-YApi](https://github.com/YMFE/yapi)*

*[YApi](http://yapi.devdemo.trs.net.cn/)*

*[YApi-Baidu](https://yapi.baidu.com/)*

*[Document](https://hellosean1025.github.io/yapi/)*

+ Deployment

  - Official Images (YApi+MongoDB)

  * MongoDB

```
$ docker pull mongo:latest
$ docker run --privileged=true -d \
    --name mongodb \
    -e MONGO_INITDB_ROOT_USERNAME=root \
    -e MONGO_INITDB_ROOT_PASSWORD=avatech \
    -e MONGO_INITDB_DATABASE=yapi \
    -v /home/admin/dockers/mongodb/mongo-conf:/docker-entrypoint-initdb.d \
    -v /home/admin/dockers/mongodb/conf:/etc/mongo \
    -v /home/admin/dockers/mongodb/data/db:/data/db \
    -p 27017:27017 \
    mongo:latest --auth

$ docker run --privileged=true -d \
    --name mongodb \
    -v /home/admin/dockers/mongodb/mongo-conf:/docker-entrypoint-initdb.d \
    -v /home/admin/dockers/mongodb/conf:/etc/mongo \
    -v /home/admin/dockers/mongodb/data/db:/data/db \
    mongo:latest --auth

$ sudo firewall-cmd --zone=public --add-port=27017/tcp --permanent
```

  * YApi

```
$ docker pull registry.cn-hangzhou.aliyuncs.com/anoy/yapi
$ docker run --privileged=true -it --rm \
  --link mongodb:mongo \
  --entrypoint npm \
  --workdir /api/vendors \
  registry.cn-hangzhou.aliyuncs.com/anoy/yapi \
  run install-server
```

  - 3rdParty Images

```
$ docker pull silsuer/yapi
$ docker run --privileged=true -dit \
    --name yapi \
    -v /home/admin/dockers/yapi/mongodb/mongo-conf:/docker-entrypoint-initdb.d \
    -v /home/admin/dockers/yapi/mongodb/conf:/etc/mongo \
    -v /home/admin/dockers/yapi/mongodb/data/db:/data/db \
    -p 27017:27017 \
    -p 9090:9090 -p 4000:3000 \
    silsuer/yapi
$ sudo firewall-cmd --zone=public --add-port=27017/tcp --add-port=4000/tcp --add-port=9090/tcp --permanent
$ docker ps
$ docker attach container-id
$ service mongodb start
$ yapi server
```

  - 3rdParty Images(Docker Compose) *Recommend*

```
$ git clone https://github.com/fjc0k/docker-YApi.git
$ vim docker-compose.yml
$ docker-compose up -d
$ sudo firewall-cmd --zone=public --add-port=9090/tcp --permanent
```

```
version: '3'

services:
  yapi-web:
    image: jayfong/yapi:latest
    container_name: yapi-web
    ports:
      - 9090:3000
    environment:
      - YAPI_ADMIN_ACCOUNT=avardd@avatech.com.cn
      - YAPI_ADMIN_PASSWORD=adm1n
      - YAPI_CLOSE_REGISTER=true
      - YAPI_DB_SERVERNAME=yapi-mongo
      - YAPI_DB_PORT=27017
      - YAPI_DB_DATABASE=yapi
      - YAPI_MAIL_ENABLE=true
      - YAPI_MAIL_HOST=smtp.qiye.aliyun.com
      - YAPI_MAIL_PORT=465
      - YAPI_MAIL_FROM=xxx@avatech.com.cn
      - YAPI_MAIL_AUTH_USER=xxx@avatech.com.cn
      - YAPI_MAIL_AUTH_PASS=xxxxxxx
      - YAPI_LDAP_LOGIN_ENABLE=false
      - YAPI_PLUGINS=[]
    depends_on:
      - yapi-mongo
    links:
      - yapi-mongo
    restart: unless-stopped
  yapi-mongo:
    image: mongo:latest
    container_name: yapi-mongo
    volumes:
      - /home/admin/dockers/yapi/mongodb/mongo-conf:/docker-entrypoint-initdb.d
      - /home/admin/dockers/yapi/mongodb/conf:/etc/mongo
      - /home/admin/dockers/yapi/mongodb/data/db:/data/db
    expose:
      - 27017
    restart: unless-stopped
```

