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
    --env=DRONE_GITEA_SERVER=http://192.168.3.14:3000/ \
    --env=DRONE_GITEA_CLIENT_ID=2c1b96ea-8c88-4f66-bf42-2f03baf93180 \
    --env=DRONE_GITEA_CLIENT_SECRET=vNDu5piicx3Ic79A9DQto4e1IMwiTQf_Pmb7JgO7ipU= \
    --env=DRONE_RPC_SECRET=16096fbe839fad18b6e060a1a05c79e7 \
    --env=DRONE_SERVER_HOST=192.168.3.14:4000 \
    --env=DRONE_SERVER_PROTO=http \
    --env=DRONE_GITEA=true \
    --publish=4000:80 \
    --publish=443:443 \
    --restart=always \
    --detach=true \
    --name=drone \
    drone/drone
  
  $ sudo firewall-cmd --zone=public --add-port=4000/tcp --permanent
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

