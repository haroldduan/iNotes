<!--
@author: harold.duan
@date: 20-09-01
@memo: Notes logging
-->

## CI/CD


  + ***[Gitea](https://github.com/go-gitea/gitea)***

  ```
  $ docker pull gitea/gitea:latest
  $ docker run --privileged=true --restart=always -d \
    -p 1022:22 -p 3000:3000 \
    -v /home/admin/dockers/gitea:/data \
    --name=gitea \
    gitea/gitea:latest
  ```

  gitea_client_id: 2c1b96ea-8c88-4f66-bf42-2f03baf93180
  gitea_client_secret: vNDu5piicx3Ic79A9DQto4e1IMwiTQf_Pmb7JgO7ipU=


  + ***[Drone](https://github.com/drone/drone)***

  ```
  $ docker pull drone/drone
  $ docker run --privileged=true --restart=always -d \
    --volume=/home/admin/dockers/drone:/data \
    --volume=/var/run/docker.sock:/var/run/docker.sock \
    --env=DRONE_GITEA=true \
    --env=DRONE_GITEA_SERVER=http://192.168.3.14:3000/ \
    --env=DRONE_GITEA_CLIENT_ID=2c1b96ea-8c88-4f66-bf42-2f03baf93180 \
    --env=DRONE_GITEA_CLIENT_SECRET=vNDu5piicx3Ic79A9DQto4e1IMwiTQf_Pmb7JgO7ipU= \
    --env=DRONE_RPC_SECRET=16096fbe839fad18b6e060a1a05c79e7 \
    --env=DRONE_SERVER_HOST=192.168.3.14:4000 \
    --env=DRONE_SERVER_PROTO=http \
    --env=DRONE_TLS_AUTOCERT=false \
    --env=DRONE_AGENTS_ENABLED=true \
    --publish=4000:80 \
    --publish=443:443 \
    --restart=always \
    --detach=true \
    --name=drone \
    drone/drone:latest

  $ docker pull drone/drone:1.2.1
  $ docker run --privileged=true --restart=always -d \
    --volume=/home/admin/dockers/drone:/data \
    --volume=/var/run/docker.sock:/var/run/docker.sock \
    --env=DRONE_GITEA=true \
    --env=DRONE_GITEA_SERVER=http://192.168.3.14:3000/ \
    --env=DRONE_GITEA_CLIENT_ID=2c1b96ea-8c88-4f66-bf42-2f03baf93180 \
    --env=DRONE_GITEA_CLIENT_SECRET=vNDu5piicx3Ic79A9DQto4e1IMwiTQf_Pmb7JgO7ipU= \
    --env=DRONE_RPC_SECRET=16096fbe839fad18b6e060a1a05c79e7 \
    --env=DRONE_SERVER_HOST=192.168.3.14:4000 \
    --env=DRONE_SERVER_PROTO=http \
    --env=DRONE_TLS_AUTOCERT=false \
    --env=DRONE_AGENTS_ENABLED=true \
    --publish=4000:80 \
    --publish=443:443 \
    --restart=always \
    --detach=true \
    --name=drone \
    drone/drone:1.2.1
  
  $ sudo firewall-cmd --zone=public --add-port=4000/tcp --permanent
  ```

  + ***[Drone Agent](https://hub.docker.com/r/drone/agent)***

  ```
  $ docker pull drone/agent
  $ docker run --privileged=true --restart=always -d \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -e DRONE_RPC_PROTO=http \
    -e DRONE_RPC_HOST=192.168.3.14:4000 \
    -e DRONE_RPC_SECRET=16096fbe839fad18b6e060a1a05c79e7 \
    -e DRONE_RUNNER_CAPACITY=2 \
    -e DRONE_RUNNER_NAME=192.169.3.14 \
    -e DRONE_LOGS_DEBUG=true \
    --env=DRONE_LOGS_TRACE=true \
    -p 5000:3000 \
    --name drone-agent \
    drone/agent:latest

  $ docker pull drone/agent:1.2.1
  $ docker run --privileged=true --restart=always -d \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -e DRONE_RPC_PROTO=http \
    -e DRONE_RPC_HOST=192.168.3.14:4000 \
    -e DRONE_RPC_SECRET=16096fbe839fad18b6e060a1a05c79e7 \
    -e DRONE_RUNNER_CAPACITY=2 \
    -e DRONE_RUNNER_NAME=192.169.3.14 \
    -e DRONE_LOGS_DEBUG=true \
    --env=DRONE_LOGS_TRACE=true \
    -p 5000:3000 \
    --name drone-agent \
    drone/agent:1.2.1
  ```

  + ***[Drone Runner](https://github.com/drone-runners/drone-runner-docker)***

  ```
  $ docker pull drone/drone-runner-docker
  $ docker run --privileged=true --restart=always -d \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -e DRONE_RPC_PROTO=http \
    -e DRONE_RPC_HOST=192.168.3.14:4000 \
    -e DRONE_RPC_SECRET=16096fbe839fad18b6e060a1a05c79e7 \
    -e DRONE_RUNNER_CAPACITY=2 \
    -e DRONE_RUNNER_NAME=192.168.3.14 \
    -p 6000:3000 \
    --name drone-runner \
    drone/drone-runner-docker:latest
  $ sudo firewall-cmd --zone=public --add-port=6000/tcp --permanent
  ```


  + ***[Vault](https://github.com/hashicorp/vault)***

  ```
  $ docker pull vault
  $ docker run --privileged=true --restart=always -d \
    --cap-add=IPC_LOCK \
    -e 'VAULT_LOCAL_CONFIG={"listener": {"tcp": {"address": "0.0.0.0:8200", "tls_disable":"1"}},"backend": {"file": {"path": "/vault/file"}}, "default_lease_ttl": "168h", "max_lease_ttl": "720h" , "ui":"true"}' \
    -v /home/admin/dockers/vault/logs:/vault/logs \
    -v /home/admin/dockers/vault/file:/vault/file \
    -p 8200:8200 \
    --name=vault \
    vault server
  
  $ sudo firewall-cmd --zone=public --add-port=8200/tcp --permanent
  ```

  *vault.json*

  ```
  {
    "keys": [
      "3c3213d6fdc05d864c36b4fbd45fc407ef262665b9133835e564fa04a9107bfb67",
      "67961c87e0ed96be9fcf416a62c92ee07ddd8004361a37b236cbabb11eb9c139bf",
      "418b9bde3e2a4a4ab52ba827c90add4fbfeb72c370a5e1593dee8794a1090ea81f",
      "e5db7cb501c474a7c692adffa5bc4ac529433eb524cb8e46d5a1dea7c4fc6b1a59",
      "2a72be78a7051e452aa28952440da77d3d36e3408e98cf95ff114f58dea94da9ca",
      "28e4014e20370bf67cb37ed7be5db9354c15e4b1594f785e53fa414f23d68b6437",
      "6ed48134249f49101653ed229d3862cd105c0c8fc5d420f2d013b214ca9885f495"
    ],
    "keys_base64": [
      "PDIT1v3AXYZMNrT71F/EB+8mJmW5Ezg15WT6BKkQe/tn",
      "Z5Ych+Dtlr6fz0FqYsku4H3dgAQ2GjeyNsursR65wTm/",
      "QYub3j4qSkq1K6gnyQrdT7/rcsNwpeFZPe6HlKEJDqgf",
      "5dt8tQHEdKfGkq3/pbxKxSlDPrUky45G1aHep8T8axpZ",
      "KnK+eKcFHkUqoolSRA2nfT0240COmM+V/xFPWN6pTanK",
      "KOQBTiA3C/Z8s37Xvl25NUwV5LFZT3heU/pBTyPWi2Q3",
      "btSBNCSfSRAWU+0inThizRBcDI/F1CDy0BOyFMqYhfSV"
    ],
    "root_token": "s.W6fgu2jVuQ8EkWEWxegfohe0"
  }
  ```

  + ***[drone/vault](https://github.com/drone/drone-vault)***

  ```
  $ docker pull drone/vault
  ```

  ```
  $ docker build -t drone-test .
  $ docker tag drone-test:latest 192.168.3.14:8083/drone-test:alpine
  $ docker push 192.168.3.14:8083/drone-test:alpine
  ```

  * Day 1 *go-test*

  ```
  kind: pipeline
  name: default
  workspace:
    base: /srv/drone-test-go
    path: .
  steps:
  - name: test
    # pull: true
    image: golang:alpine
    commands:
      - pwd
      - ls
      - CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o demo .
      - ./demo
      - echo "Shit"
  ```

  * Day 2 *node-test*

  ```
  kind: pipeline
  type: docker
  name: default
  steps:
    - name: publish  
      image: plugins/docker
      settings:
        registry: 192.168.3.14:8083
        repo: 192.168.3.14:8083/avardc/drone-test-node
        username: admin
        password: 
          from_secret: reg_passwd
        insecure: true
        tags: alpine
        dockerfile: ./Dockerfile
    - name: deploy
      image: appleboy/drone-ssh
      settings:
        host: 192.168.3.14
        port:
          22
          # command_timeout: 2m
        username: admin
        password:
          from_secret: ssh_passwd
        script:
          - echo "Remove old image..."
          - docker rmi -f 192.168.3.14:8082/avardc/drone-test-node:alpine
          - echo "Pull image..."
          - docker pull 192.168.3.14:8082/avardc/drone-test-node:alpine
          - echo "Run docker..."
          - docker run -p 13000:13000 --name=test-drone-node -d 192.168.3.14:8082/avardc/drone-test-node:alpine
  ```

  * Day 3 *node-test*

  ```
  kind: pipeline
  type: docker
  name: default

  steps:
    # - name: publish  
    #   image: plugins/docker
    #   settings:
    #     registry: 192.168.3.14:8083
    #     repo: 192.168.3.14:8083/avardc/drone-test-node
    #     username: admin
    #     password: 
    #       from_secret: reg_passwd
    #     insecure: true
    #     tags: alpine
    #     dockerfile: ./Dockerfile
    # - name: deploy
    #   image: appleboy/drone-ssh
    #   settings:
    #     host: 192.168.3.14
    #     port:
    #       22
    #       # command_timeout: 2m
    #     username: admin
    #     password:
    #       from_secret: ssh_passwd
    #     script:
    #       - echo "Remove old docker ps..."
    #       - docker stop test-drone-node
    #       - docker rm test-drone-node -f
    #       # - echo "Remove old image..."
    #       # - docker rmi -f 192.168.3.14:8082/avardc/drone-test-node:alpine
    #       - echo "Pull image..."
    #       - docker pull 192.168.3.14:8082/avardc/drone-test-node:alpine
    #       - echo "Run docker..."
    #       - docker run -p 13000:13000 --name=test-drone-node -d 192.168.3.14:8082/avardc/drone-test-node:alpine
    - name: deploy
      image: appleboy/drone-ssh
      settings:
        host: 192.168.3.14
        port:
          22
          # command_timeout: 2m
        username: admin
        password:
          from_secret: ssh_passwd
        script:
          - echo "Build image..."
          - docker build -t avardc/drone-test-node:alpine .
          - echo "Tag image..."
          - docker tag avardc/drone-test-node:alpine 192.168.3.14:8083/drone-test:alpine
          - echo "Login registory..."
          - docker login 192.168.3.14:8083 -u admin -p $reg_passwd
          - docker push 192.168.3.14:8083/avardc/drone-test-node:alpine
          - echo "Remove old docker ps..."
          - docker stop test-drone-node
          - docker rm test-drone-node -f
          # - echo "Remove old image..."
          # - docker rmi -f 192.168.3.14:8082/avardc/drone-test-node:alpine
          - echo "Pull image..."
          - docker pull 192.168.3.14:8082/avardc/drone-test-node:alpine
          - echo "Run docker..."
          - docker run -p 13000:13000 --name=test-drone-node -d 192.168.3.14:8082/avardc/drone-test-node:alpine
  # - name: test
  #   image: node:latest
  #   commands:
  #   - npm install
  #   - npm start

  # - name: docker
  #   image: plugins/docker
  #   settings:
  #     username: admin
  #     password: *****
  #     repo: codebot/test-drone-node
  #     dockerfile: path/to/Dockerfile
  ```

  * Day 4 *python-test*

  ```
  kind: pipeline
  type: docker
  name: default

  steps:
    - name: publish  
      image: plugins/docker
      settings:
        registry: 192.168.3.14:8083
        repo: 192.168.3.14:8083/avardc/drone-test-python
        username: admin
        password: 
          from_secret: reg_passwd
        insecure: true
        tags: alpine
        dockerfile: ./Dockerfile
    - name: deploy
      image: appleboy/drone-ssh
      settings:
        host: 192.168.3.14
        port:
          22
          # command_timeout: 2m
        username: admin
        password:
          from_secret: ssh_passwd
        script:
          # - echo "Build image..."
          # - docker build -t avardc/drone-test-python:alpine .
          # - echo "Tag image..."
          # - docker tag avardc/drone-test-python:alpine 192.168.3.14:8083/avardc/drone-test-python:alpine
          # - echo "Login registory..."
          # - docker login 192.168.3.14:8083 -u admin -p $reg_passwd
          # - docker push 192.168.3.14:8083/avardc/drone-test-python:alpine
          - echo "Remove old docker ps..."
          - docker stop drone-test-python
          - docker rm drone-test-python -f
          # - echo "Remove old image..."
          # - docker rmi -f 192.168.3.14:8082/avardc/drone-test-node:alpine
          - echo "Pull image..."
          - docker pull 192.168.3.14:8082/avardc/drone-test-python:alpine
          - echo "Run docker..."
          - docker run --privileged=true -p 10080:80 --name=drone-test-python -d 192.168.3.14:8082/avardc/drone-test-python:alpine
  ```

## Automated Testing

### Macaca

  + ***[DataHub](https://github.com/macacajs/macaca-datahub)***

  ```
  $ docker pull macacajs/macaca-datahub
  $ docker run --privileged=true \
    --name macaca-datahub \
    -v /home/admin/dockers/macaca-datahub:/root/.macaca-datahub \
    -p 9200:9200 \
    -p 9300:9300 \
    -d macacajs/macaca-datahub
  ```

### OnlyOffice

  + ***[DocumentServer](https://github.com/ONLYOFFICE/DocumentServer)***

  ```
  $ docker pull onlyoffice/documentserver
  $ docker run --privileged=true -d -p 8080:80 \
      --name onlyoffice \
      -v /home/admin/dockers/onlyoffice/DocumentServer/logs:/var/log/onlyoffice  \
      -v /home/admin/dockers/onlyoffice/DocumentServer/data:/var/www/onlyoffice/Data \
      onlyoffice/documentserver
  $ docker run --privileged=true -d -p 8080:80 \
      --name onlyoffice \
      -v /home/admin/dockers/onlyoffice/DocumentServer/logs:/var/log/onlyoffice  \
      -v /home/admin/dockers/onlyoffice/DocumentServer/data:/var/www/onlyoffice/Data  \
      -v /home/admin/dockers/onlyoffice/DocumentServer/lib:/var/lib/onlyoffice \
      -v /home/admin/dockers/onlyoffice/DocumentServer/rabbitmq:/var/lib/rabbitmq \
      -v /home/admin/dockers/onlyoffice/DocumentServer/redis:/var/lib/redis \
      -v /home/admin/dockers/onlyoffice/DocumentServer/db:/var/lib/postgresql \
      onlyoffice/documentserver
  ```

  ```
  $ docker network create --driver bridge onlyoffice
  ```

  + **STEP 1:** **Install ONLYOFFICE Document Server*

  ```
  $ docker pull onlyoffice/documentserver
  $ docker run --privileged=true --net onlyoffice -i -t -d --restart=always \
    --name onlyoffice-document-server \
    -v /home/admin/dockers/onlyoffice/DocumentServer/data:/var/www/onlyoffice/Data \
    -v /home/admin/dockers/onlyoffice/DocumentServer/logs:/var/log/onlyoffice \
    onlyoffice/documentserver
  ```

  + **STEP 2:** *Install ONLYOFFICE Mail Server*

  ```
  $ docker pull onlyoffice/mailserver
  $ docker run --privileged=true --net onlyoffice --privileged -i -t -d --restart=always \
    --name onlyoffice-mail-server \
    -p 2525:25 -p 143:143 -p 587:587 \
    -v /home/admin/dockers/onlyoffice/MailServer/data:/var/vmail \
    -v /home/admin/dockers/onlyoffice/MailServer/data/certs:/etc/pki/tls/mailserver \
    -v /home/admin/dockers/onlyoffice/MailServer/logs:/var/log \
    -v /home/admin/dockers/onlyoffice/MailServer/mysql:/var/lib/mysql \
    -h 192.168.3.14 \
    onlyoffice/mailserver
  ```

  + **STEP 3:** *Install ONLYOFFICE Community Server*

  ```
  $ docker pull onlyoffice/communityserver
  $ docker run --privileged=true --net onlyoffice -i -t -d --restart=always \
    --name onlyoffice-community-server \
    -p 8080:80 -p 5222:5222 -p 1443:443 \
    -v /home/admin/dockers/onlyoffice/CommunityServer/data:/var/www/onlyoffice/Data \
    -v /home/admin/dockers/onlyoffice/CommunityServer/mysql:/var/lib/mysql \
    -v /home/admin/dockers/onlyoffice/CommunityServer/logs:/var/log/onlyoffice \
    -v /home/admin/dockers/onlyoffice/DocumentServer/data:/var/www/onlyoffice/DocumentServerData \
    -e DOCUMENT_SERVER_PORT_80_TCP_ADDR=onlyoffice-document-server \
    -e MAIL_SERVER_DB_HOST=onlyoffice-mail-server \
    onlyoffice/communityserver
  ```
