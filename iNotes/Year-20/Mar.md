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

$ docker pull jogetworkflow/joget-enterprise
$ docker volume create jogetdata
$ docker run -d --name jogetdb -p 3306:3306 -e MYSQL_ROOT_PASSWORD=jwdb -e MYSQL_USER=joget -e MYSQL_PASSWORD=joget -e MYSQL_DATABASE=jwdb mysql:5.7
$ docker run -d --link jogetdb:jwdb --name joget -p 8080:8080 -e MYSQL_HOST=jwdb -e MYSQL_DATABASE=jwdb -e MYSQL_PORT=3306 -e MYSQL_USER=joget -e MYSQL_PASSWORD=joget --mount source=jogetdata,target=/opt/joget/wflow jogetworkflow/joget-community
```

+ Git

```
$ sudo yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
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
