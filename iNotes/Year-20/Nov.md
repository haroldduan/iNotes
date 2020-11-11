<!--
@author: harold.duan
@date: 20-11-01
@memo: Notes logging
-->

### Redash

*Optional Item*

``` clone Redash repo
$ git clone https://github.com/getredash/redash
```

``` edit docker compose version 9.0 exists error
# This is an example configuration for Docker Compose. Make sure to atleast update
# the cookie secret & postgres database password.
#
# Some other recommendations:
# 1. To persist Postgres data, assign it a volume host location.
# 2. Split the worker service to adhoc workers and scheduled queries workers.
version: '2'
services:
  server:
    image: redash/redash:9.0.0-beta.b42121
    command: server
    depends_on:
      - postgres
      - redis
    ports:
      - "5050:5000"
    environment:
      PYTHONUNBUFFERED: 0
      REDASH_LOG_LEVEL: "INFO"
      REDASH_REDIS_URL: "redis://redis:6379/0"
      REDASH_DATABASE_URL: "postgresql://postgres@postgres/postgres"
      REDASH_COOKIE_SECRET: "Q422k6vaXUk8"
      REDASH_WEB_WORKERS: 4
      # email settings
      REDASH_MAIL_SERVER: "smtp.qiye.aliyun.com"
      REDASH_MAIL_PORT: 465
      REDASH_MAIL_USE_TLS: "false"
      REDASH_MAIL_USE_SSL: "true"
      REDASH_MAIL_USERNAME: "xxx@avatech.com.cn"
      REDASH_MAIL_PASSWORD: "xxxxxxxx"
      REDASH_MAIL_DEFAULT_SENDER: "xxx@avatech.com.cn"
      REDASH_HOST: "http://192.168.3.14:8080"
    restart: always
  worker:
    image: redash/redash:9.0.0-beta.b42121
    command: scheduler
    environment:
      PYTHONUNBUFFERED: 0
      REDASH_LOG_LEVEL: "INFO"
      REDASH_REDIS_URL: "redis://redis:6379/0"
      REDASH_DATABASE_URL: "postgresql://postgres@postgres/postgres"
      QUEUES: "queries,scheduled_queries,celery"
      WORKERS_COUNT: 2
    restart: always
  redis:
    image: redis:3.0-alpine
    ports:
    - "6379:6379"
    volumes: 
      - /home/admin/dockers/redash/data/redis_data:/data
    restart: always
  postgres:
    image: postgres:9.5.6-alpine
    ports:
    - "5432:5432"
    volumes:
      - /home/admin/dockers/redash/data/postgresql_data:/var/lib/postgresql/data
    restart: always
  nginx:
    image: redash/nginx:latest
    ports:
      - "8080:80"
    depends_on:
      - server
    links:
      - server:redash
    restart: always
```

``` edit docker compose version 3.0
version: '2'
services:
  server:
    image: redash/redash:3.0.0.b2998
    command: server
    depends_on:
      - postgres
      - redis
    ports:
      - "5000:5000"
    environment:
      PYTHONUNBUFFERED: 0
      REDASH_LOG_LEVEL: "INFO"
      REDASH_REDIS_URL: "redis://redis:6379/0"
      REDASH_DATABASE_URL: "postgresql://postgres@postgres/postgres"
      REDASH_COOKIE_SECRET: "123456"
      REDASH_WEB_WORKERS: 4
  worker:
    image: redash/redash:3.0.0.b2998
    command: scheduler
    environment:
      PYTHONUNBUFFERED: 0
      REDASH_LOG_LEVEL: "INFO"
      REDASH_REDIS_URL: "redis://redis:6379/0"
      REDASH_DATABASE_URL: "postgresql://postgres@postgres/postgres"
      QUEUES: "queries,scheduled_queries,celery"
      WORKERS_COUNT: 2
  redis:
    image: redis:3.0-alpine
  mongodb:
    image: mongo:3.2
    ports:
    - "27017:27017"
  click:
     image: yandex/clickhouse-server
     ports:
     - "8123:8123"
     - "8000:9000"
     - "8009:9009"
     ulimits:
      nproc: 65535
      nofile:
       soft: 262144
       hard: 262144
  postgres:
    image: postgres:9.5.6-alpine
    ports:
    - "5432:5432"
    volumes:
    - /home/admin/dockers/redash/data/postgresql_data:/var/lib/postgresql/data
  nginx:
    image: redash/nginx:latest
    ports:
      - "8080:80"
    depends_on:
      - server
    links:
      - server:redash
```

``` docker run
$ docker-compose run --rm server create_db
$ docker-compose up -d
```

### Flutter

**on Windows**

1. Installing Dart

[Dart for Windows](https://gekorm.com/dart-windows/)

2. Install Flutter SDK

+ Flutter SDK install

[Flutter SDK releases](https://flutter.dev/docs/development/tools/sdk/releases#windows)

> 去flutter官网下载其最新可用的安装包，点击下载 ；  
> 注意，Flutter的渠道版本会不停变动，请以Flutter官网为准。另外，在中国大陆地区，要想正常获取安装包列表或下载安装包，可能需要翻墙，也可以去Flutter github项目下去下载安装包 。  
> 将安装包zip解压到你想安装Flutter SDK的路径（如：C:\Android\flutter；注意，不要将flutter安装到需要一些高权限的路径如C:\Program Files\）。  
> 在Flutter安装目录的flutter文件下找到flutter_console.bat，双击运行并启动flutter命令行，接下来，你就可以在Flutter命令行运行flutter命令了。  

*Tips*

注意： 由于一些flutter命令需要联网获取数据，如果您是在国内访问，由于众所周知的原因，直接访问很可能不会成功。 上面的PUB_HOSTED_URL和FLUTTER_STORAGE_BASE_URL是google为国内开发者搭建的临时镜像。详情请参考 [Using Flutter in China](https://github.com/flutter/flutter/wiki/Using-Flutter-in-China)

+ ENV Update

> 要在终端运行 flutter 命令， 你需要添加以下环境变量到系统PATH：  
> 转到 “控制面板>用户帐户>用户帐户>更改我的环境变量”  
> 在“用户变量(系统变量)”下检查是否有名为“Path”的条目:  
> 如果该条目存在, 追加 flutter\bin的全路径，使用 ; 作为分隔符.  
> 如果条目不存在, 创建一个新用户变量 Path ，然后将 flutter\bin的全路径作为它的值.  
> 在“用户变量”下检查是否有名为”PUB_HOSTED_URL”和”FLUTTER_STORAGE_BASE_URL”的条目，如果没有，也添加它们。  

*Tips*

由于在国内访问Flutter有时可能会受到限制，Flutter官方为中国开发者搭建了临时镜像，大家可以将如下环境变量加入到用户环境变量中：

```
PUB_HOSTED_URL=https://pub.flutter-io.cn
FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```

*运行 flutter doctor*

```
$ flutter doctor
```

3. Android设置

 + 安装Android Studio

> 要为Android开发Flutter应用，您可以使用Mac，Windows或Linux（64位）机器.  
> Flutter需要安装和配置Android Studio:  
> 下载并安装 [Android Studio](https://developer.android.com/studio/index.html).  
> 启动Android Studio，然后执行“Android Studio安装向导”。这将安装最新的Android SDK，Android SDK平台工具和Android SDK构建工具，这是Flutter为Android开发时所必需的  
> 设置Android设备  
> 要准备在Android设备上运行并测试您的Flutter应用，您需要安装Android 4.1（API level 16）或更高版本的Android设备.  

*Tips*

***如果运行flutter doctor 命令出现 Android toolchain 缺失***

在环境变量中设置Android SDK路径，如下：

```
ANDROID_HOME=C:\Android\Sdk
```

4. Installing VSCode Plugins

[VSCode Dart](https://github.com/Dart-Code/Dart-Code.git)

[VSCode Flutter](https://github.com/Dart-Code/Flutter.git)

*Usefulling URLs*

https://www.dartcn.com/

https://dart.cn/

https://flutter.cn/docs/get-started/install/windows

https://flutterchina.club/get-started/test-drive/#vscode

https://www.yiibai.com/dart/

https://zhuanlan.zhihu.com/p/53672286

https://blog.csdn.net/kabuto_hui/article/details/79505262

https://pub.flutter-io.cn/