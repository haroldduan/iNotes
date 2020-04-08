<!--
@author: harold.duan
@date: 20-03-01
@memo: Notes logging
-->

## RDS DevOps

+ Portainer

```
$ docker run -p 9000:9000 \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /home/git/portainer:/data \
    -v /etc/localtime:/etc/localtime:ro \
    --name portainer \
    -d docker.io/portainer/portainer
```

+ Gitea

```
$ docker run --privileged=true --restart=always -d \
    --network=gitea-net --network-alias=gitea \
    --ip=172.20.0.2 \
    --name=gitea -p 1022:22 -p 7070:3000 \
    -v /etc/localtime:/etc/localtime:ro \
    -v /home/git/gitea:/data gitea/gitea:1.11.2
```

+ Code-Server

```
$ docker pull codercom/code-server
$ docker run -d -p 8080:8080 \
    -v /home/admin/coder/project:/home/coder/project \
    --name=code-server codercom/code-server
$ docker run -d -p 8080:8080 \
    -v "$PWD:/home/admin/coder/project" \
    --name=code-server codercom/code-server
$ docker run -dit --restart=always \
    --name vscode -h vscode \
    -u root  -p 8080:8080 \
    -v /home/admin/vscode:/root \
    -e PASSWORD=avatech \
    -v /etc/localtime:/etc/localtime:ro \
    codercom/code-server  --auth password

$ vim docker-compose.yml
```

+ Locust

```
$ docker run --name=locust -d \
    -v /home/admin/locust/reports:/opt/reports \
    -v /home/admin/locust/.aws:/root/.aws \
    -v /home/admin/locust/script:/opt/script \
    -v /home/admin/locust/credentials:/meta/credentials \
    -p 8089:8089 \
    -e ROLE=standalone \
    -e TARGET_URL=http://192.168.3.14.8089 \
    -e LOCUST_FILE=https://raw.githubusercontent.com/zalando-incubator/docker-locust/master/example/simple.py \
    -e SLAVE_MUL=4 \
    -e AUTOMATIC=False \
    locustio/locust

$ docker run -d -p 8089:8089 \
    --name=locust \
    --volume /home/admin/locustfile:/mnt/locust \
    -e TARGET_URL=https://www.baidu.com \
    -e LOCUSTFILE_PATH=/mnt/locust/locustfile.py \
    locustio/locust

# locustfile.py

from locust import HttpLocust, TaskSet, task

# 定义用户行为
class UserBehavior(TaskSet):
 
    @task
    def baidu_index(self):
        self.client.get("/")
 
class WebsiteUser(HttpLocust):
    task_set = UserBehavior
    min_wait = 3000
    max_wait = 6000
```

+ Syncthing

```
$ docker pull syncthing/syncthing
$ docker run -d --name=syncthing \
     -p 8384:8384 -p 22000:22000 \
     -v /home/admin/syncthing:/var/syncthing \
     syncthing/syncthing:latest
```

+ Joget

```
$ docker pull jogetworkflow/joget-community
$ docker run -d -p 7070:8080 -v /var/lib/mysql --name joget jogetworkflow/joget-community
docker run -d -p 3000:8080 -v /var/lib/mysql --name joget jogetworkflow/joget-community
$ docker pull jogetworkflow/joget-enterprise
$ docker volume create jogetdata
$ docker run -d --name jogetdb -p 3306:3306 -e MYSQL_ROOT_PASSWORD=jwdb -e MYSQL_USER=joget -e MYSQL_PASSWORD=joget -e MYSQL_DATABASE=jwdb mysql:5.7
$ docker run -d --link jogetdb:jwdb --name joget -p 8080:8080 -e MYSQL_HOST=jwdb -e MYSQL_DATABASE=jwdb -e MYSQL_PORT=3306 -e MYSQL_USER=joget -e MYSQL_PASSWORD=joget --mount source=jogetdata,target=/opt/joget/wflow jogetworkflow/joget-community
```

+ Git

```
$ sudo yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
$ sudo yum install gcc-c++
$ sudo yum install  gcc perl-ExtUtils-MakeMaker
$ wget -c https://github.com/git/git/archive/v2.25.1.tar.gz
$ tar -zxvf v2.25.1.tar.gz
$ cd git-2.25.1
$ make prefix=/usr/local/git all
$ make prefix=/usr/local/git install
$ su root
$ echo "export PATH=$PATH:/usr/local/git/bin" >> /etc/bashrc
$ source /etc/bashrc
```

```

$ sudo yum install \
https://repo.ius.io/ius-release-el7.rpm \
https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
$ sudo yum install git2u
```

+ go-convey

```
$ go get github.com/smartystreets/goconvey
```

+ go cover

```
# $ go get code.google.com/p/go.tools/cmd/cover
$ go get golang.org/x/tools/cmd/cover
```

+ NextCloud install

```
$ docker pull nextcloud
$ docker run -d \
  -v /home/admin/dockers/nextcloud:/var/www/html \
  -p 80:80 \
  --name nextcloud \
  nextcloud
$ # docker run -d \
  # -v /home/admin/dockers/nextcloud:/var/www/html \
  # -v apps:/var/www/html/custom_apps \
  # -v config:/var/www/html/config \
  # -v data:/var/www/html/data \
  # -p 80:80 \
  # --name nextcloud \
  # nextcloud
```

+ CapRover

```
sudo firewall-cmd --zone=public --add-port=3000/tcp --permanent
sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent
sudo firewall-cmd --zone=public --add-port=7946/tcp --permanent
sudo firewall-cmd --zone=public --add-port=4789/tcp --permanent
sudo firewall-cmd --zone=public --add-port=2377/tcp --permanent
sudo firewall-cmd --zone=public --add-port=996/tcp --permanent
sudo firewall-cmd --zone=public --add-port=7946/udp --permanent
sudo firewall-cmd --zone=public --add-port=4789/udp --permanent
sudo firewall-cmd --zone=public --add-port=2377/udp --permanent

$ docker pull dockersaturn/captainduckduck
$ sudo mkdir /captain 
$ docker run -e BY_PASS_PROXY_CHECK='TRUE' -p 8080:80 -p 1443:443 -p 3000:3000 -v /var/run/docker.sock:/var/run/docker.sock --name caprover dockersaturn/captainduckduck

$ docker run -e "MAIN_NODE_IP_ADDRESS=114.113.221.167" -p 8080:80 -p 1443:443 -p 3000:3000 -v /var/run/docker.sock:/var/run/docker.sock --name caprover dockersaturn/captainduckduck
```

## Docker UI

+ Portainer

```
$ docker pull docker.io/portainer/portainer
$ docker run -p 9999:9000 \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /home/admin/dockers/portainer:/data \
    --name portainer \
    -d docker.io/portainer/portainer
```

```
swarmpit/install:1.8
$ docker pull swarmpit/install:edge
$ docker run -it --rm \
  --name swarmpit-installer \
  --volume /var/run/docker.sock:/var/run/docker.sock \
  swarmpit/install:edge
```

## Docker Machine

+ Install

```
$ base=https://github.com/docker/machine/releases/download/v0.16.0 &&
  curl -L $base/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine &&
  sudo mv /tmp/docker-machine /usr/local/bin/docker-machine &&
  chmod +x /usr/local/bin/docker-machine
```

+ Usage

```
$ docker-machine version
$ docker-machine ls
$ docker-machine create --driver virtualbox test
```



+ Wekan

```
# pull MongoDB
$ docker pull mongo:4.0
$ docker run -d --restart=always --name wekan-db mongo:4.0

# pull Wekan
$ docker pull quay.io/wekan/wekan
$ docker run -d --restart=always \
  --name wekan \
  --link "wekan-db:db" \
  -e "WITH_API=true" \
  -e "MONGO_URL=mongodb://db" \
  -e "ROOT_URL=http://192.168.3.14:3000" \
  -p 3000:8080 \
  quay.io/wekan/wekan 
```

+ Redmine

```
$ docker pull redmine
$ docker run -d --name redmine \
  -p 3000:3000 \
  redmine
```

+ Drone

```
$ docker run -d \
  --volume=/home/admin/dockers/drone:/data \
  --volume=/var/run/docker.sock:/var/run/docker.sock \
  --env=DRONE_GITEA_SERVER=http://182.92.165.53:7070/ \
  --env=DRONE_GITEA_CLIENT_ID=1f7f34d5-b4fc-4b93-bb93-a3fc979dd952 \
  --env=DRONE_GITEA_CLIENT_SECRET=z1DIMS0lmAOlGX8mi3fOEqT9qhWXD8_CbSiiJluG2Dw= \
  --env=DRONE_RPC_SECRET=e94722bddc0c5b612ca26025b95cb5e7 \
  --env=DRONE_SERVER_HOST=114.113.221.167:3000 \
  --env=DRONE_SERVER_PROTO=http \
  --env=DRONE_GITEA=true \
  --publish=3000:80 \
  --publish=443:443 \
  --restart=always \
  --detach=true \
  --name=drone \
  drone/drone
```

```
$ drone-server:
    image: drone/drone:0.7
    ports:
      - 8381:8000
    volumes:
      - drone-db:/var/lib/drone/
    restart: always
    environment:
      - DRONE_OPEN=true
      - DRONE_HOST=http://drone-server:8381
      - DRONE_SECRET=test
      - DRONE_GITEA=true
      - DRONE_GITEA_URL=http://gitea-server:3000/
      - DRONE_NETWORK=${DEFAULT_NETWORK}
    depends_on:
      - gitea-server

  drone-agent:
    image: drone/drone:0.7
    command: agent
    restart: always
    depends_on:
      - drone-server
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - DRONE_SERVER=ws://drone-server:8000/ws/broker
      - DRONE_SECRET=test
```

$ sudo yum install -y epel-release
$ sudo rpm -ivh https://centos7.iuscommunity.org/ius-release.rpm
$ sudo yum list git2u
$ sudo yum install -y git2u
```

``` awesome method
$ sudo yum install \
https://repo.ius.io/ius-release-el7.rpm \
https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

$ sudo yum list git2*
$ sudo yum install git222
```