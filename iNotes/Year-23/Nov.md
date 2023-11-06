<!--
@author: harold.duan
@date: 23-11-02
@memo: Notes logging
-->

```
$ docker pull gitea/gitea:1.20.5

# update app.ini
$ cd /home/git/docker-data/gitea/gitea/conf
$ vim app.ini
```

***Old app.ini:***
    [server]
    APP_DATA_PATH=/data/gitea
    ...
    LFS_START_SERVER=true
    LFS_CONTENT_PATH=/data/git/lfs
    LFS_JWT_SECRET=xxx

    [mailer]
    ENABLED = true
    HOST = smtp.qiye.aliyun.com:465
    FROM = avardd@avatech.com.cn
    USER = avardd@avatech.com.cn
    PASSWD = xxxxx

***New app.ini:***
    [lfs]
    PATH=/data/git/lfs

    [server]
    APP_DATA_PATH=/data/gitea
    ...
    LFS_START_SERVER=true
    LFS_JWT_SECRET=xxx
    LOCAL_ROOT_URL = https://rds.avacloud.com.cn:7070/

    [mailer]
    ENABLED = true
    ; HOST = smtp.qiye.aliyun.com:465
    PROTOCOL = smtps
    SMTP_ADDR = smtp.qiye.aliyun.com
    SMTP_PORT = 465
    FROM = avardd@avatech.com.cn
    USER = avardd@avatech.com.cn
    PASSWD = xxxxx

```
# Exists bugs version 1.20.5
$ docker run --privileged=true --restart=always -d \
    --network=gitea-net --network-alias=gitea \
    --ip=172.20.0.2 \
    -p 1022:22 -p 7070:3000 \
    -v /home/git/docker-data/gitea:/data \
    -v /home/git/certs/gitea:/certs \
    --name=gitea \
    gitea/gitea:1.20.5

$ docker run --privileged=true --restart=always -d \
    --network=portainer-net --network-alias=portainer \
    --ip=172.18.0.2 \
    -p 9443:9443 -p 9000:8000 \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /home/git/docker-data/portainer:/data \
    -v /home/git/certs/apache:/certs \
    --name portainer \
    portainer/portainer-ce:2.19.1 \
    --sslcert /certs/rds.avacloud.com.cn.crt \
    --sslkey /certs/rds.avacloud.com.cn.key
```