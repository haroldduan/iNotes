<!--
@author: harold.duan
@date: 19-12-01
@memo: Notes logging
-->

# ShowDoc

## ShowDoc install in docker

```
$ mkdir showdoc showdoc/html
$ chmod -R 777 showdoc/
# $ docker pull star7th/showdoc:v2.4.17
$ docker pull star7th/showdoc
$ docker network create showdoc-net
$ docker run --restart=always --privileged=true --name=showdoc --network showdoc-net --network-alias showdoc -d -p 9090:80 -v /home/git/showdoc/html:/var/www/html/  star7th/showdoc
```

**Error process**

+ ***docker: Error response from daemon: driver failed programming external connectivity on endpoint showdoc (0c46ef5d9fea4dfe91278992cf814ea34d3cf96f4d539b959c2a5d53893f15ae):  (iptables failed: iptables --wait -t nat -A DOCKER -p tcp -d 0/0 --dport 9090 -j DNAT --to-destination 172.17.0.4:80 ! -i docker0: iptables: No chain/target/match by that name.***

*solution:* ***Restart docker***

```
$ sudo service docker restart
```
