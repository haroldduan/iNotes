<!--
@author: harold.duan
@date: 23-06-09
@memo: Notes logging
-->

+ **FileBrowser**

```
$ cd /home/git/docker-data
$ mkdir filebrowser
$ cd filebrowser

# $ touch database.db
# $ su root
# $ chmod -R 755 database.db

$ docker pull filebrowser/filebrowser

$ sudo docker run --privileged=true --restart=always -d \
    -p 7000:80 \
    --name=filebrowser \
    filebrowser/filebrowser

$ su root
$ docker cp filebrowser:/database.db .
$ docker cp filebrowser:/.filebrowser.json .
$ chmod -R 755 database.db .filebrower.json

$ sudo docker run --privileged=true --restart=always -d \
    -v /home/git/docker-data/filebrowser/srv:/srv \
    -v /home/git/docker-data/filebrowser/database.db:/database.db \
    -v /home/git/docker-data/filebrowser/.filebrowser.json:/.filebrowser.json \
    -v /home/git/docker-data/filebrowser/filebrowser.log:/var/log/filebrowser.log \
    -v /home/git/certs/filebrowser/rds.avacloud.com.cn.crt:/ssl.crt \
    -v /home/git/certs/filebrowser/rds.avacloud.com.cn.key:/ssl.key \
    -p 7000:80 \
    --name=filebrowser \
    filebrowser/filebrowser
```

sudo docker run --privileged=true --restart=always -d \
    -v /home/git/docker-data/filebrowser/srv:/srv \
    -v /home/git/docker-data/filebrowser/database.db:/database.db \
    -v /home/git/docker-data/filebrowser/.filebrowser.json:/.filebrowser.json \
    -p 7000:80 \
    --name=filebrowser \
    filebrowser/filebrowser

sudo docker run --privileged=true --restart=always -d \
    -p 7000:80 \
    --name=filebrowser \
    filebrowser/filebrowser

filebrowser -d /filebrowser.db config set --cert ssl.crt --key ssl.key