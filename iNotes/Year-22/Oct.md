<!--
@author: harold.duan
@date: 22-10-03
@memo: Notes logging
-->

## RDS Upgrade

  + **Potainer**

  ```
  $ docker pull rds.avacloud.com.cn:8082/portainer/portainer-ce:latest
  docker pull portainer/portainer-ce:2.15.1

  $ docker run --privileged=true --restart=always -d \
    --network=portainer-net --network-alias=portainer \
    --ip=172.18.0.2 \
    -p 9000:9000 \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /home/git/docker-data/portainer:/data \
    --name portainer \
    rds.avacloud.com.cn:8082/portainer/portainer-ce:latest

  $ docker run --privileged=true --restart=always -d \
    --network=portainer-net --network-alias=portainer \
    --ip=172.18.0.2 \
    -p 9443:9443 -p 8000:8000 \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /home/git/docker-data/portainer:/data \
    -v /home/git/certs/apache:/certs \
    --name portainer \
    rds.avacloud.com.cn:8082/portainer/portainer-ce:latest \
    --sslcert /certs/rds.avacloud.com.cn.crt \
    --sslkey /certs/rds.avacloud.com.cn.key

    docker run --privileged=true --restart=always -d \
    --network=portainer-net --network-alias=portainer \
    --ip=172.18.0.2 \
    -p 9443:9443 -p 9000:8000 \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /home/git/docker-data/portainer:/data \
    -v /home/git/certs/apache:/certs \
    --name portainer \
    portainer/portainer-ce:2.15.1 \
    --ssl \
    --sslcert /certs/rds.avacloud.com.cn.crt \
    --sslkey /certs/rds.avacloud.com.cn.key

  $ sudo firewall-cmd --zone=public --add-port=9443/tcp --permanent
  ```

  + **Gitea**

  ```
  $ docker pull rds.avacloud.com.cn:8082/gitea/gitea:latest
  docker pull gitea/gitea:1.17.2

  $ docker run --privileged=true --restart=always -d \
    --network=gitea-net --network-alias=gitea \
    --ip=172.20.0.2 \
    -p 1022:22 -p 7070:3000 \
    -v /home/git/docker-data/gitea:/data \
    --name=gitea \
    rds.avatech.com.cn:8082/gitea/gitea:latest

    docker run --privileged=true --restart=always -d \
    --network=gitea-net --network-alias=gitea \
    --ip=172.20.0.2 \
    -p 1022:22 -p 7070:3000 \
    -v /home/git/docker-data/gitea:/data \
    -v /home/git/certs/gitea:/certs \
    --name=gitea \
    gitea/gitea:1.17.2
  
  $ docker run --privileged=true --restart=always -d \
    --network=portainer-net --network-alias=portainer \
    --ip=172.18.0.2 \
    -p 9443:9443 -p 9000:8000 \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /home/git/docker-data/portainer:/data \
    -v /home/git/certs/apache:/certs \
    --name portainer \
    rds.avacloud.com.cn:8082/portainer/portainer-ce:latest \
    --sslcert /certs/rds.avacloud.com.cn.crt \
    --sslkey /certs/rds.avacloud.com.cn.key
  ```