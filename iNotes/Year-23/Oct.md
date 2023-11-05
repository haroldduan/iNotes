<!--
@author: harold.duan
@date: 23-10-04
@memo: Notes logging
-->

```
# $ sudo openssl x509 -inform PEM -in rds.avacloud.com.cn.crt -out rds.avacloud.com.cn.cert
$ sudo yum install java-1.8.0-openjdk-devel

$ cd /home/git/docker-data/nexus/nexus-data/etc
$ mkdir ssl
$ keytool -genkeypair -keystore keystore.jks -storepass avatech -keypass avatech -alias jetty -keyalg RSA -keysize 2048 -validity 5000 -dname "CN=*.rds.avacloud.com.cn, OU=Example, O=Sonatype, L=Unspecified, ST=Unspecified, C=US" -ext "SAN=DNS:rds.avacloud.com.cn,IP:39.106.1.85" -ext "BC=ca:true"

# 修改$data-dir/etc/nexus.properties文件，在第一行添加application-port-ssl=8443
修改$data-dir/etc/nexus.properties文件，修改Key为nexus-args所在行的值，在后面添加,${jetty.etc}/jetty-https.xml,${jetty.etc}/jetty-http-redirect-to-https.xml

$ cd ..
$ vim nexus.properties

# old
#################################################################################################
# Jetty section
# application-port=8081
# application-host=0.0.0.0
# nexus-args=${jetty.etc}/jetty.xml,${jetty.etc}/jetty-http.xml,${jetty.etc}/jetty-requestlog.xml
# nexus-context-path=/${NEXUS_CONTEXT}

# Nexus section
# nexus-edition=nexus-pro-edition
# nexus-features=\
#  nexus-pro-feature
# nexus.clustered=false
#################################################################################################

# new
#################################################################################################
# Jetty section
# application-port-ssl=8443
# application-port=8081
# application-host=0.0.0.0
# nexus-args=${jetty.etc}/jetty.xml,${jetty.etc}/jetty-http.xml,${jetty.etc}/jetty-requestlog.xml,${jetty.etc}/jetty-https.xml,${jetty.etc}/jetty-http-redirect-to-https.xml
# nexus-context-path=/${NEXUS_CONTEXT}

# Nexus section
# nexus-edition=nexus-pro-edition
# nexus-features=\
#  nexus-pro-feature
# nexus.clustered=false
#################################################################################################

# 修改${jetty.etc}/jetty-https.xml文件中keystore和truststore的配置部分
# mkdir jetty
# cd jetty
# 
<Set name="KeyStorePath"><Property name="ssl.etc"/>/keystore.jks</Set>
<Set name="KeyStorePassword">avatech</Set>
<Set name="KeyManagerPassword">avatech</Set>
<Set name="TrustStorePath"><Property name="ssl.etc"/>/keystore.jks</Set>
<Set name="TrustStorePassword">avatech</Set>

$ docker pull sonatype/nexus3:latest
$ docker run --privileged=true --restart=always -d \
    --network=nexus-net --network-alias=nexus \
    --ip=172.25.0.2 \
    -p 8443:8443 -p 8081:8081 -p 8082:8082 -p 8083:8083 \
    -v /home/git/docker-data/nexus/nexus-data:/nexus-data \
    --name=nexus \
    sonatype/nexus3:latest

# In docker command
$ cd /opt/sonatype/nexus/etc/ssl
$ keytool -genkeypair -keystore keystore.jks -storepass avatech -keypass avatech -alias jetty -keyalg RSA -keysize 2048 -validity 5000 -dname "CN=*.rds.avacloud.com.cn, OU=Example, O=Sonatype, L=Unspecified, ST=Unspecified, C=US" -ext "SAN=DNS:rds.avacloud.com.cn,IP:39.106.1.85" -ext "BC=ca:true"

$ docker run --privileged=true --restart=always -d \
    --network=nexus-net --network-alias=nexus \
    --ip=172.25.0.2 \
    -p 8443:8443 -p 8081:8081 -p 8082:8082 -p 8083:8083 \
    -v /home/git/docker-data/nexus/nexus-data:/nexus-data \
    -v /home/git/docker-data/nexus/nexus-data/etc/ssl:/nexus-data/opt/sonatype/nexus/etc/ssl \
    -v /home/git/docker-data/nexus/nexus-data/etc/nexus.properties:/nexus-data/opt/sonatype/nexus/etc/nexus-default.properties \
    -v /home/git/docker-data/nexus/nexus-data/etc/jetty/jetty-https.xml:/nexus-data/opt/sonatype/nexus/etc/jetty/jetty-https.xml \
    --name=nexus \
    sonatype/nexus3:latest

$ docker run --privileged=true --restart=always -d \
    --network=nexus-net --network-alias=nexus \
    --ip=172.25.0.2 \
    -p 8443:8443 -p 8081:8081 -p 8082:8082 -p 8083:8083 \
    -v /home/git/docker-data/nexus/nexus-data:/nexus-data \
    -v /home/git/docker-data/nexus/nexus-data/etc:/opt/sonatype/nexus/etc \
    --name=nexus \
    sonatype/nexus3:latest

```