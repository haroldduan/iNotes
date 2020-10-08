<!--
@author: harold.duan
@date: 20-10-01
@memo: Notes logging
-->

## RDS Upgrade

  + **Portainer**

  ```
  $ docker pull rds.avatech.com.cn:8082/portainer/portainer

  $ docker run --privileged=true --restart=always -d \
    --network=portainer-net --network-alias=portainer \
    --ip=172.18.0.2 \
    -p 9000:9000 \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /home/git/docker-data/portainer:/data \
    --name portainer \
    rds.avatech.com.cn:8082/portainer/portainer
  ```

  + **Gitea**

  ```
  $ docker pull rds.avatech.com.cn:8082/gitea/gitea:latest

  $ docker run --privileged=true --restart=always -d \
    --network=gitea-net --network-alias=gitea \
    --ip=172.20.0.2 \
    -p 1022:22 -p 7070:3000 \
    -v /home/git/docker-data/gitea:/data \
    --name=gitea \
    rds.avatech.com.cn:8082/gitea/gitea:latest
  ```

  + **Nexus3**

  ```
  $ docker pull rds.avatech.com.cn:8082/sonatype/nexus3:latest

  $ docker network create nexus-net --subnet=172.25.0.0/16
  
  $ docker run --privileged=true --restart=always -d \
    --network=nexus-net --network-alias=nexus \
    --ip=172.25.0.2 \
    -p 8081:8081 -p 8082:8082 -p 8083:8083 \
    -v /home/git/docker-data/nexus/nexus-data:/nexus-data \
    --name=nexus \
    rds.avatech.com.cn:8082/sonatype/nexus3:latest
  ```

  + **Nextcloud**

  ```
  $ docker pull rds.avatech.com.cn:8082/nextcloud:latest

  $ docker run --privileged=true --restart=always -d \
    -p 10080:80 \
    -v /home/git/docker-data/nextcloud:/var/www/html \
    --name=nextcloud \
    rds.avatech.com.cn:8082/nextcloud
  ```

  + **Drone**

  ClientID:ba71bc70-c651-47d4-9672-8048f5d6d1d3
  ClientSecret:MqyecgFXwkYnCwr2tZMB3HXiU9cHNfwzlCwq5QUoR3w=

  ```
  # $ docker pull rds.avatech.com.cn:8082/drone/drone
  $ docker pull drone/drone
  $ docker run \
    --privileged=true --restart=always -d \
    --network=drone-net --network-alias=drone \
    --ip=172.22.0.2 \
    --volume=/var/run/docker.sock:/var/run/docker.sock \
    --volume=/home/git/docker-data/drone:/data \
    --env=DRONE_GITEA=true \
    --env=DRONE_GITEA_SERVER=http://rds.avatech.com.cn:7070 \
    --env=DRONE_GITEA_CLIENT_ID=ba71bc70-c651-47d4-9672-8048f5d6d1d3 \
    --env=DRONE_GITEA_CLIENT_SECRET=MqyecgFXwkYnCwr2tZMB3HXiU9cHNfwzlCwq5QUoR3w= \
    --env=DRONE_SERVER_HOST=rds.avatech.com.cn:5050 \
    --env=DRONE_RPC_SECRET=16096fbe839fad18b6e060a1a05c79e7 \
    --env=DRONE_USER_CREATE=username:codebot,admin:false \
    --env=DRONE_SERVER_PROTO=http \
    --env=DRONE_TLS_AUTOCERT=false \
    --env=DRONE_AGENTS_ENABLED=true \
    --publish=5050:80 \
    --publish=443:443 \
    --restart=always \
    --name=drone \
    drone/drone:latest

  $ docker run \
    --privileged=true --restart=always -d \
    --network=drone-net --network-alias=drone \
    --ip=172.22.0.2 \
    --volume=/var/run/docker.sock:/var/run/docker.sock \
    --volume=/home/git/docker-data/drone:/data \
    --env=DRONE_GITEA=true \
    --env=DRONE_GITEA_SERVER=http://rds.avatech.com.cn:7070 \
    --env=DRONE_GITEA_CLIENT_ID=ba71bc70-c651-47d4-9672-8048f5d6d1d3 \
    --env=DRONE_GITEA_CLIENT_SECRET=MqyecgFXwkYnCwr2tZMB3HXiU9cHNfwzlCwq5QUoR3w= \
    --env=DRONE_SERVER_HOST=rds.avatech.com.cn:5050 \
    --env=DRONE_RPC_SECRET=16096fbe839fad18b6e060a1a05c79e7 \
    --env=DRONE_USER_CREATE=username:codebot,admin:true \
    --env=DRONE_SERVER_PROTO=http \
    --env=DRONE_TLS_AUTOCERT=false \
    --env=DRONE_AGENTS_ENABLED=true \
    --publish=5050:80 \
    --publish=443:443 \
    --restart=always \
    --name=drone \
    drone/drone:latest
  ```

  + **Drone Agent**

  ```
  # $ docker pull rds.avatech.com.cn:8082/drone/agent
  $ docker pull drone/agent
  $ docker run --privileged=true --restart=always -d \
    -v /var/run/docker.sock:/var/run/docker.sock \
    --network=drone-net --network-alias=drone-agent \
    --ip=172.22.0.3 \
    -e DRONE_RPC_PROTO=http \
    -e DRONE_RPC_HOST=rds.avatech.com.cn:5050 \
    -e DRONE_RPC_SECRET=16096fbe839fad18b6e060a1a05c79e7 \
    -e DRONE_RUNNER_CAPACITY=2 \
    -e DRONE_RUNNER_NAME=rds.avatech.com.cn \
    -e DRONE_LOGS_DEBUG=true \
    --env=DRONE_LOGS_TRACE=true \
    -p 5000:3000 \
    --name drone-agent \
    drone/agent:latest
  ```