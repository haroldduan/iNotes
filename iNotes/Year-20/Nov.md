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