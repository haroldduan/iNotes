<!--
@author: harold.duan
@date: 21-03-01
@memo: Notes logging
-->

## filebrowser

### Installing

```
$ docker pull filebrowser/filebrowser
$ sudo mkdir filebrowser
$ cd filebrowser
$ sudo touch filebrowser.db .filebrowser.json
$ docker run --privileged=true --restart=always -d \
    -v /root:/srv \
    -v /home/admin/dockers/filebrowser/filebrowser.db:/database.db \
    -v /home/admin/dockers/filebrowser/.filebrowser.json:/.filebrowser.json \
    --name filebrowser \
    -p 7080:80 \
    filebrowser/filebrowser
$ sudo touch filebrowser.db
$ docker run --privileged=true --restart=always -d \
    -v /home/admin/filebrowser:/srv \
    -v /home/admin/dockers/filebrowser/filebrowser.db:/database.db \
    --name filebrowser \
    -p 7080:80 \
    filebrowser/filebrowser
```