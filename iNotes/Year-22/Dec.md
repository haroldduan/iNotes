<!--
@author: harold.duan
@date: 22-12-31
@memo: Notes logging
-->

## RDS Upgrade

  + **NextCloud**

  ```
  $ docker pull nextcloud
  $ docker run --privileged=true -d \
    -v /home/git/docker-data/nextcloud:/var/www/html \
    -v /home/git/docker-data/nextcloud/apps:/var/www/html/custom_apps \
    -v /home/git/docker-data/nextcloud/config:/var/www/html/config \
    -v /home/git/docker-data/nextcloud/data:/var/www/html/data \
    -p 3000:80 \
    --name nextcloud \
	nextcloud:latest

  $ docker pull onlyoffice/documentserver
  $ docker run --privileged=true \
    -i -t -d -p 3030:80 --restart=always \
    --name onlyoffice-document-server \
    -v /home/git/docker-data/onlyoffice/DocumentServer/logs:/var/log/onlyoffice \
    -v /home/git/docker-data/onlyoffice/DocumentServer/data:/var/www/onlyoffice/Data \
    -v /home/git/docker-data/onlyoffice/DocumentServer/lib:/var/lib/onlyoffice \
    -v /home/git/docker-data/onlyoffice/DocumentServer/db:/var/lib/postgresql \
    -e JWT_ENABLED=true \
    -e JWT_SECRET=avatech \
    onlyoffice/documentserver
  ```