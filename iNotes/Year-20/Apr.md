<!--
@author: harold.duan
@date: 20-04-01
@memo: Notes logging
-->

## Docker kits

### Docker Compose

**[Docker-Compose](https://github.com/docker/compose)**

> 简单来讲，Compose是用来定义和运行一个或多个容器应用的工具。使用compaose可以简化容器镜像的建立及容器的运行。
> Compose使用python语言开发，非常适合在单机环境里部署一个或多个容器，并自动把多个容器互相关联起来。  
> Compose 中有两个重要的概念：  
> 服务 (service)：一个应用的容器，实际上可以包括若干运行相同镜像的容器实例。  
> 项目 (project)：由一组关联的应用容器组成的一个完整业务单元，在 docker-compose.yml 文件中定义。  
> Compose是使用YML文件来定义多容器应用的，它还会用 docker-compose up 命令把完整的应用运行起来。docker-compose up 命令为应用的运行做了所有的准备工作。从本质上讲，Compose把YML文件解析成docker命令的参数，然后调用相应的docker命令行接口，从而把应用以容器化的方式管理起来。它通过解析容器间的依赖关系来顺序启动容器。而容器间的依赖关系则可以通过在 docker-compose.yml文件中使用 links 标记指定。

### Docker Machine

**[Docker-Machine](https://github.com/docker/machine/)**

> Docker Machine 是 Docker 官方编排（Orchestration）项目之一，负责在多种平台上快速安装 Docker 环境。  
> Docker Machine 项目基于 Go 语言实现，目前在 Github 上进行维护。  

### Docker Swarm

> Docker Swarm 是 Docker 官方三剑客项目之一，提供 Docker 容器集群服务，是 Docker 官方对容器云生态进行支持的核心方案。使用它，用户可以将多个 Docker 主机封装为单个大型的虚拟 Docker 主机，快速打造一套容器云平台。  
> Docker 1.12 Swarm mode 已经内嵌入 Docker 引擎，成为了 docker 子命令 docker swarm。请注意与旧的 Docker Swarm 区分开来。  
> Swarm mode内置kv存储功能，提供了众多的新特性，比如：具有容错能力的去中心化设计、内置服务发现、负载均衡、路由网格、动态伸缩、滚动更新、安全传输等。使得 Docker 原生的 Swarm 集群具备与 Mesos、Kubernetes 竞争的实力。  

> Swarm 是使用 SwarmKit 构建的 Docker 引擎内置（原生）的集群管理和编排工具。

**[SwarmKit](https://github.com/docker/swarmkit/)**

**节点：**

*运行 Docker 的主机可以主动初始化一个 Swarm 集群或者加入一个已存在的 Swarm 集群，这样这个运行 Docker 的主机就成为一个 Swarm 集群的节点 (node)*  
*节点分为管理 (manager) 节点和工作 (worker) 节点*  
*管理节点：用于 Swarm 集群的管理，docker swarm 命令基本只能在管理节点执行（节点退出集群命令 docker swarm leave 可以在工作节点执行）。一个 Swarm 集群可以有多个管理节点，但只有一个管理节点可以成为 leader，leader 通过 raft 协议实现。*  
*工作节点：是任务执行节点，管理节点将服务 (service) 下发至工作节点执行。管理节点默认也作为工作节点。也可以通过配置让服务只运行在管理节点。* 

**服务和任务：**

*任务 （Task）是 Swarm 中的最小的调度单位，目前来说就是一个单一的容器。*  
*服务 （Services） 是指一组任务的集合，服务定义了任务的属性。服务有两种模式：*  
*replicated services 按照一定规则在各个工作节点上运行指定个数的任务。*  
*global services 每个工作节点上运行一个任务。*  
*两种模式通过 docker service create 的 --mode 参数指定。*  

**swarm：**

*从 v1.12 开始，集群管理和编排功能已经集成进 Docker Engine。当 Docker Engine 初始化了一个 swarm 或者加入到一个存在的 swarm 时，它就启动了 swarm mode。没启动 swarm mode 时，Docker 执行的是容器命令；运行 swarm mode 后，Docker 增加了编排 service 的能力。*  
*Docker 允许在同一个 Docker 主机上既运行 swarm service，又运行单独的容器。*  

**node：**

*swarm 中的每个 Docker Engine 都是一个 node，有两种类型的 node：manager 和 worker。*  
*为了向 swarm 中部署应用，我们需要在 manager node 上执行部署命令，manager node 会将部署任务拆解并分配给一个或多个worker node 完成部署。manager node 负责执行编排和集群管理工作，保持并维护 swarm 处于期望的状态。swarm 中如果有多个 manager node，它们会自动协商并选举出一个 leader 执行编排任务。woker node 接受并执行由 manager node 派发的任务。默认配置下 manager node 同时也是一个 worker node，不过可以将其配置成 manager-only node，让其专职负责编排和集群管理工作。work node 会定期向 manager node 报告自己的状态和它正在执行的任务的状态，这样 manager 就可以维护整个集群的状态。*  

**service：**

*service 定义了 worker node 上要执行的任务。swarm 的主要编排任务就是保证 service 处于期望的状态下。*  
*举一个 service 的例子：在 swarm 中启动一个 http 服务，使用的镜像是 httpd:latest，副本数为 3。manager node 负责创建这个 service，经过分析知道需要启动 3 个 httpd 容器，根据当前各 worker node 的状态将运行容器的任务分配下去，比如 worker1 上运行两个容器，worker2 上运行一个容器。运行了一段时间，worker2 突然宕机了，manager 监控到这个故障，于是立即在 worker3 上启动了一个新的 httpd 容器。这样就保证了 service 处于期望的三个副本状态。*




1. Removing old versions

```
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

2. Installing docker and kits

```
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2

$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
#$ sudo yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
$ sudo yum install docker-ce docker-ce-cli containerd.io
```

3. Configuration

```
$ sudo service docker start
$ sudo systemctl enable docker
# Output contents
#Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
$ sudo groupadd docker
$ sudo gpasswd -a $USER docker
$ newgrp docker
$ sudo vim /etc/docker/daemon.json
# input this contents
{
  "registry-mirrors":
  [
    "https://registry.docker-cn.com",
    "https://docker.mirrors.ustc.edu.cn",
    "http://hub-mirror.c.163.com"
  ],
  "debug": true,
  "log-level": "info",
  "graph": "/home/admin/docker-data",
  "hosts": ["tcp://0.0.0.0:2375", "unix:///var/run/docker.sock"]
}
$ sudo service docker restart
```

*Docker 服务文件路径：Ubuntu的路径 /lib/systemd/system/docker.service，CentOS的路径 /usr/lib/systemd/system/docker.service*

```
$ sudo vim /usr/lib/systemd/system/docker.service

# Update contents:

# ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
ExecStart=/usr/bin/dockerd
# ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock
```

4. Docker 集群操作

    4.1. 选举集群manager节点

    *(On testsrv-15)*

    ```
    $ docker swarm init --advertise-addr 192.168.3.15
    ```

    Output this contents

    ```
    Swarm initialized: current node (6mbb3owo4ykaz1m6xr0h8ifz5) is now a manager.

    To add a worker to this swarm, run the following command:

        docker swarm join --token SWMTKN-1-2kdjrjf3gxb8ll0eei18fjl5yk2x2yfvnerx797w784hgsl0fy-aoq69x9fjmut4qm0pz9j6v61q 192.168.3.15:2377

    To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
    ```

    **Tips**

    If lost the join commander,can input this commander

    *(On testsrv-15)*

    ```
    $ docker swarm join-token manager
    # or
    #$ docker swarm join-token worker
    ```

    ***工作节点和管理节点的接入命令中使用的接入 Token（SWMTKN...）是不同的***

    4.2. 查看节点信息

    *(On testsrv-15)*

    ```
    $  docker swarm leave
    ```
    ```
    ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
    6mbb3owo4ykaz1m6xr0h8ifz5 *   testsrv-15          Ready               Active              Leader              19.03.8
    ```

    4.3. 将其他节点加入集群 

    *(On testsrv-16)*

    ```
    $ docker swarm join --token SWMTKN-1-2kdjrjf3gxb8ll0eei18fjl5yk2x2yfvnerx797w784hgsl0fy-aoq69x9fjmut4qm0pz9j6v61q 192.168.3.15:2377
    ```

    **Errors**
    
    + Join node failed,"connect: no route to host"

    > Error response from daemon: rpc error: code = Unavailable desc = all SubConns are in TransientFailure, latest connection error: connection error: desc = "transport: Error while dialing dial tcp 192.168.3.15:2377: connect: no route to host"

    *Solution*

    > **Open protocols and ports between the hosts**  
    > The following ports must be available. On some systems, these ports are open by default.  
    > + TCP port 2377 for cluster management communications  
    > + TCP and UDP port 7946 for communication among nodes  
    > + UDP port 4789 for overlay network traffic  

    ***2377/tcp：用于客户端与 Swarm 进行安全通信。***
    ***7946/tcp 与 7946/udp：用于控制面 gossip 分发。***
    ***4789/udp：用于基于 VXLAN 的覆盖网络。***

    *(On testsrv-15)*

    ```
    #$ sudo firewall-cmd --zone=public --add-port=2376/tcp --permanent
    $ sudo firewall-cmd --zone=public --add-port=2377/tcp --permanent
    $ sudo firewall-cmd --zone=public --add-port=7946/tcp --permanent
    $ sudo firewall-cmd --zone=public --add-port=7946/udp --permanent
    $ sudo firewall-cmd --zone=public --add-port=4789/udp --permanent

    $ sudo firewall-cmd --reload
    $ sudo firewall-cmd --list-all
    ```

    *(On testsrv-16)*

    ```
    $ docker swarm join --token SWMTKN-1-2kdjrjf3gxb8ll0eei18fjl5yk2x2yfvnerx797w784hgsl0fy-aoq69x9fjmut4qm0pz9j6v61q 192.168.3.15:2377
    ```

    + Join node failed,"certificate has expired or is not yet valid"

    > Error response from daemon: error while validating Root CA Certificate: x509: certificate has expired or is not yet valid

    *Reason*

    > docker swarm node之间是加密传输，docker swarm init时会创建CA证书。这里提示证书过期，可能是主机当前时间不对。

    *Solution*

    > 在各自宿主机上同步一下时间

    *(On testsrv-15 and testsrv-16)*

    ```
    # show time
    $ date
    # 或者通过显示系统时间配置
    $ timedatectl status
    # 根据服务器输出的时间对比，发现是15服务器(testsrv-15)时间不对，因为15服务器在安装时并未开启同步网络时间
    ```

    *(On testsrv-15)*

    ```
    # install ntp ntpdate
    $ sudo yum -y install ntp ntpdate
    # 方法一，设置系统时间同步于ntp时间
    $ sudo timedatectl set-ntp true
    # 方法二，手动同步系统时间
    $ sudo ntpdate cn.pool.ntp.org
    ```

    *(On testsrv-16)*

    ```
    $ docker swarm join --token SWMTKN-1-2kdjrjf3gxb8ll0eei18fjl5yk2x2yfvnerx797w784hgsl0fy-aoq69x9fjmut4qm0pz9j6v61q 192.168.3.15:2377
    ```

    **Success**

    *Output this contents*

    ```
    This node joined a swarm as a worker.
    ```


    *(On testsrv-15)*

    ``` 
    $ docker node ls
    ```

    ```
    ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
    6mbb3owo4ykaz1m6xr0h8ifz5 *   testsrv-15          Ready               Active              Leader              19.03.8
    fw0jgf9aa4xuyjxf7k5e263pa     testsrv-16          Ready               Active                                  19.03.8
    ```

    4.4. 离开Swarm
    ``` 
    $ docker swarm leave
    ```

    ```
    $ docker info
    ```

    ```
    ...
    Swarm: active
      NodeID: 6mbb3owo4ykaz1m6xr0h8ifz5
      Is Manager: true
      ClusterID: vepl1sb8g5pk6ev2z7ifhm92g
      Managers: 1
      Nodes: 2
      Default Address Pool: 10.0.0.0/8
      SubnetSize: 24
      Data Path Port: 4789
      Orchestration:
        Task History Retention Limit: 5
      Raft:
        Snapshot Interval: 10000
        Number of Old Snapshots to Retain: 0
        Heartbeat Tick: 1
        Election Tick: 10
      Dispatcher:
        Heartbeat Period: 5 seconds
      CA Configuration:
        Expiry Duration: 3 months
        Force Rotate: 0
      Autolock Managers: false
      Root Rotation In Progress: false
      Node Address: 192.168.3.15
      Manager Addresses:
      192.168.3.15:2377
    ...
    ```

5. Portainer Installing

```
$ docker pull docker.io/portainer/portainer

# $ docker run -p 9000:9000 \
    -p 8000:8000 \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /home/admin/dockers/portainer:/data \
    --name portainer \
    -d docker.io/portainer/portainer \
    -H unix:///var/run/docker.sock
$ docker run -d -p 9000:9000 --restart=always --name portainer \
  -v /home/admin/dockers/portainer:/data \
  docker.io/portainer/portainer \
  -H tcp://192.168.3.15:2375
$ docker run -d -p 9000:9000 --restart=always --name portainer \
  -v /home/admin/dockers/portainer:/data \
  docker.io/portainer/portainer
```

*Docker API 端口开放*

```
$ sudo firewall-cmd --zone=public --add-port=2375/tcp --permanent
```

```
$ docker pull codercom/code-server
$ docker run -dit --restart=always \
    --name vscode -h vscode \
    -u root  -p 8080:8080 \
    -v /home/admin/dockers/vscode:/root \
    -e PASSWORD=avatech \
    -v /etc/localtime:/etc/localtime:ro \
    codercom/code-server  --auth password
```

6. 测试

  *(On testsrv-15)*

  6.1. 在一个工作节点上创建一个名为 helloworld 的服务，这里是随机指派给一个工作节点

  ```
  $ docker service create --replicas 1 --name helloworld alpine ping docker.com

  # Output this content:
  qi6e6j0r19yl9ejaq7fi614gq
  overall progress: 1 out of 1 tasks
  1/1: running   [==================================================>]
  verify: Service converged
  ```

  6.2. 查看 helloworld 服务运行在哪个节点上

  ```
  $ docker service ps helloworld

  # Output this content:
  ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE                ERROR               PORTS
  g19pts9cajma        helloworld.1        alpine:latest       testsrv-15          Running             Running about a minute ago
  ```

  6.3. 查看 helloworld 部署的具体信息

  ```
  $ docker service inspect --pretty helloworld

  # Output this content:
  ID:             qi6e6j0r19yl9ejaq7fi614gq
  Name:           helloworld
  Service Mode:   Replicated
    Replicas:      1
  Placement:
  UpdateConfig:
    Parallelism:   1
    On failure:    pause
    Monitoring Period: 5s
    Max failure ratio: 0
    Update order:      stop-first
  RollbackConfig:
    Parallelism:   1
    On failure:    pause
    Monitoring Period: 5s
    Max failure ratio: 0
    Rollback order:    stop-first
  ContainerSpec:
    Image:         alpine:latest@sha256:b276d875eeed9c7d3f1cfa7edb06b22ed22b14219a7d67c52c56612330348239
    Args:          ping docker.com
    Init:          false
  Resources:
  Endpoint Mode:  vip
  ```

  6.4. 扩展集群服务，将上述的 helloworld 服务扩展到俩个节点。

  ```
  $ docker service scale helloworld=2

  # Output this content:
  helloworld scaled to 2
  overall progress: 2 out of 2 tasks
  1/2: running   [==================================================>]
  2/2: running   [==================================================>]
  verify: Service converged
  ```

  6.5. 删除服务

  ```
  $ docker service rm helloworld
  # Tips,删除服务后，由此服务创建的任务容器会自动删除

  # Show services
  $ docker service ls
  ```


  6.6. 滚动升级服务

  ```
  $ docker service create --replicas 1 --name redis --update-delay 10s redis:3.0.6

  $ docker service update --image redis:3.0.7 redis
  ```

  6.7. 停止某个节点接收新的任务

  ```
  # Show nodes
  $ docker node ls

  # Drain node
  $ docker node update --availability drain testsrv-16

  # Active node
  $ docker node update --availability active testsrv-16
  ```


7. Docker Registry installing

*(On testsrv-15)*

+ By standalone docker

```
$ docker pull registry
$ docker run -d \
  -v /home/admin/dockers/registry:/var/lib/registry \
  -p 5000:5000 --restart=always \
  --name registry \
  registry:latest

# http address:http://192.168.3.15:5000/v2/_catalog
```

*(On testsrv-16)*

```
# Vist registry by curl
$ curl http://192.168.3.15:5000/v2/_catalog
# Output this content:
{"repositories":[]}

$ docker pull busybox
$ docker images
# Output this content:
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
busybox             latest              be5888e67be6        2 days ago          1.22MB

$ docker tag busybox:latest 192.168.3.15:5000/busybox:v1
$ docker images
# Output this content:
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
192.168.3.15:5000/busybox   v1                  be5888e67be6        2 days ago          1.22MB
busybox                     latest              be5888e67be6        2 days ago          1.22MB

$ docker push 192.168.3.15:5000/busybox:v1
# Output this content:
The push refers to repository [192.168.3.15:5000/busybox]
Get https://192.168.3.14:5000/v2/: http: server gave HTTP response to HTTPS client
```

> The push refers to repository [192.168.3.15:5000/busybox]  
> Get https://192.168.3.14:5000/v2/: http: server gave HTTP response to HTTPS client

*Reason*

> docker registry默认需要https的方法才能上传，可以修改下daemon.json来解决：

*Solution*

> 添加私有镜像服务器的地址到daemon.json

```
$ sudo vim /etc/docker/daemon.json

# Input this content:
"insecure-registries": [ "192.168.3.15:5000"]

$ sudo service docker restart

$ docker push 192.168.3.15:5000/busybox:v1
# Output this content:
The push refers to repository [192.168.3.15:5000/busybox]
5b0d2d635df8: Pushed
v1: digest: sha256:a2490cec4484ee6c1068ba3a05f89934010c85242f736280b35343483b2264b6 size: 527

$ curl http://192.168.3.15:5000/v2/_catalog
# Output this content:
{"repositories":["busybox"]}

curl  http://192.168.3.15:5000/v2/busybox/tags/list
# Output this content:
{"name":"busybox","tags":["v1"]}

$ docker pull 192.168.3.15:5000/busybox:v1
# Output this content:
v1: Pulling from busybox
e2334dd9fee4: Pull complete
Digest: sha256:a2490cec4484ee6c1068ba3a05f89934010c85242f736280b35343483b2264b6
Status: Downloaded newer image for 192.168.3.15:5000/busybox:v1
192.168.3.15:5000/busybox:v1

$ docker images
# Output this content:
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
192.168.3.15:5000/busybox   v1                  be5888e67be6        2 days ago          1.22MB

```

8. Portus installing

+ Docker-Compose installing

```
$ sudo yum install python3
$ pip3 install docker-compose --user
```

+ Portus installing

```
$ git clone https://github.com/SUSE/Portus.git
$ cd Portus

```

9. Sonatype-Nexus3 installing

*(On testsrv-15)*

```
$ docker pull sonatype/nexus3
$ mkdir /home/admin/dockers/nexus
$ mkdir /home/admin/dockers/nexus/nexus-data
$ sudo chown -R 200 /home/admin/dockers/nexus

$ docker run -d \
  -p 8081:8081 \
  -p 5000:5000 \
  -v /home/admin/dockers/nexus/nexus-data:/nexus-data \
  --name nexus \
  sonatype/nexus3

# Init password
# /opt/sonatype/nexus3/...
```

+ Nexus-Docker

*(On testsrv-16)*

```
$ docker login -u admin -p AVAtech@2020 192.168.3.14:5000

# Output this content:
WARNING! Using --password via the CLI is insecure. Use --password-stdin.
WARNING! Your password will be stored unencrypted in /home/admin/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded

$ docker tag busybox:latest 192.168.3.15:5000/busybox:v1
```

> WARNING! Using --password via the CLI is insecure. Use --password-stdin.  
> Error response from daemon: Get http://192.168.3.15:5000/v2/: dial tcp 192.168.3.15:5000: connect: connection refused

*Reason*

在Nexus中设置的DockerRegistry所需的5000端口未在Nexus容器启动时开放

```
$ docker run -d \
  -p 8081:8081 \
  -p 5000:5000 \ #此端口映射打开
  -v /home/admin/dockers/nexus/nexus-data:/nexus-data \
  --name nexus \
  sonatype/nexus3
```

> WARNING! Using --password via the CLI is insecure. Use --password-stdin.  
> Error response from daemon: login attempt to http://192.168.3.15:5000/v2/ failed with status: 401 Unauthorized

*Reason*

Nexus中的realms未配置

*Solution*

在Nexus中配置realms（不配置此项无法进行login,pull,push）

**Docker Bearer Token Realm** active

> [DEPRECATION NOTICE] registry v2 schema1 support will be removed in an upcoming release. Please contact admins of the 192.168.3.14:5000 registry NOW to avoid future disruption. More information at https://docs.docker.com/registry/spec/deprecated-schema-v1/  
> error parsing HTTP 404 response body: unexpected end of JSON input: ""

*Reason*

5000所对应的Registry为Group类型镜像，只能进行Pull,不能进行Push

*Solution*

为docker-hosted的Registry开放端口(8083),此端口既可以Pull也可以Push

*(On testsrv-15)*

```
$ docker run -d \
  -p 8081:8081 \
  -p 5000:5000 \ #此端口映射打开pull
  -p 8083:8083 \ #此端口映射打开push
  -v /home/admin/dockers/nexus/nexus-data:/nexus-data \
  --name nexus \
  sonatype/nexus3

$ docker tag 192.168.3.15:5000/busybox:latest 192.168.3.14:8083/busybox:v1
$ docker push 192.168.3.15:8083/busybox:v1
# Output this content:
5b0d2d635df8: Pushed
v1: digest: sha256:a2490cec4484ee6c1068ba3a05f89934010c85242f736280b35343483b2264b6 size: 527
```

+ Nexus-PyPi

*(On testsrv-16)*

```
$ pip3 install cactus -i http://192.168.3.15:8081/repository/pypi-group/simple/  --trusted-host 192.168.3.15
$ pip3 install twine --index http://192.168.3.15:8081/repository/pypi-group/pypi/  --trusted-host 192.168.3.15
```

+ Nexus-npm

*(On testsrv-16)*

```
$ npm install cnpm -g --registry=http://192.168.3.15:8081/repository/npm-group/
```

***整理汇总***

DockerRegistry-Group:组镜像源，聚合DockerRegistry-Hosted 与 DockerRegistry-Proxy,只能进行Pull操作，可自动拉取代理镜像源上的镜像；  
DockerRegistry-Hosted:私有(本地)镜像源，可进行Pull(允许匿名)和Push操作;  
DockerRegistry-Proxy：代理镜像源，可使用外部镜像进行代理，只能进行Pull操作;

docker-hosted port:8083  
docker-proxy port:8082  

```
$ docker run -d \
  --restart=always \
  -p 8081:8081 \
  -p 8082:8082 \
  -p 8083:8083 \
  -v /home/admin/dockers/nexus/nexus-data:/nexus-data \
  --name nexus \
  sonatype/nexus3
```

***Tips***

+ > Docker Swarm 是 Docker 的集群管理工具。它将 Docker 主机池转变为单个虚拟 Docker 主机。 Docker Swarm 提供了标准的 Docker API，所有任何已经与 Docker 守护程序通信的工具都可以使用 Swarm 轻松地扩展到多个主机。  
  > 支持的工具包括但不限于以下各项：  
  > Dokku<br>Docker Compose<br>Docker Machine<br>Jenkins  

+ > swarm 集群由管理节点（manager）和工作节点（work node）构成。  
  > swarm mananger：负责整个集群的管理工作包括集群配置、服务管理等所有跟集群有关的工作。  
  > work node：主要负责运行相应的服务来执行任务（task）。

+ > 集群管理有关的任何操作，都是在管理节点上操作的。

+ > docker info 可以查看当前集群的信息。

+ > + docker swarm：集群管理，子命令有 init, join,join-token, leave, update  
  > + docker node：节点管理，子命令有 demote, inspect,ls, promote, rm, ps, update  
  > + docker service：服务管理，子命令有 create, inspect, ps, ls ,rm , scale, update  


## Open source Registry kits

Name|Address
---|---
Docker-Registry|[Registry](https://github.com/docker-archive/docker-registry)
Portus|[Portus](https://github.com/SUSE/Portus)
Harbor|[Harbor](https://github.com/goharbor/harbor)
Sonatype-Nexus3|[Sonatype Nexus](https://github.com/sonatype/docker-nexus3)

## Open source Docker UI kits

Name|Address
---|---
Portainer|[Portainer](https://github.com/portainer/portainer)
DockerUI|[DockerUI](https://github.com/kevana/ui-for-docker)
Shipyard|[Shipyard](https://github.com/shipyard/shipyard)


## Trello

## n8n

```
$ docker pull n8nio/n8n

$ docker run -d --rm \
  --name n8n \
  -p 5678:5678 \
  -v /home/admin/dockers/n8n/n8n-data:/root/.n8n \
  n8nio/n8n
```

## Rust

+ Rust installing

```
$ curl https://sh.rustup.rs -sSf | sh
# or
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Output this contents
...
1) Proceed with installation (default)
2) Customize installation
3) Cancel installation
...

$ source $HOME/.cargo/env

$ rustc --version
$ cargo --version
```

+ Rust kits installing

```
$ rustup install nightly
$ rustup default nightly
```

```
$ cargo install racer
$ racer -V
$ cargo install --force rustfmt
$ cargo install --force rls
```

+ Rust components installing

```
$ rustup component add rls-preview rust-analysis rust-src
```

+ VSCode plugin installing

```
$ ext install rust-lang.rust

$ ext install vadimcn.vscode-lldb
```

+ Debug testing

```
$ cargo build
# edit launch.json
```


+ ~~Update repository (新版本可以不配置)~~

```
# vim ~/.cargo/config 

[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"
replace-with = 'ustc'
[source.ustc]
registry = "git://mirrors.ustc.edu.cn/crates.io-index"
```

``` fish
# vim $HOME/.config/fish/config.fish

export RUSTUP_DIST_SERVER="https://mirrors.ustc.edu.cn/rust-static"
export RUSTUP_UPDATE_ROOT="https://mirrors.ustc.edu.cn/rust-static/rustup"
export PATH="$HOME/.cargo/bin:$PATH
```