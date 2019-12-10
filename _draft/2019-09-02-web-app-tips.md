---
title: "웹 모니터링 개발 기록(web monitoring system dev note)"
date: 2019-12-10T10:00:00+09:00
categories:
  - 기록
tags:
  - web
  - monitoring
  - javascript
  - react
  - GraphQL
  - flask
  - python
  - docker
  - docker-compose
---

### 웹 모니터링 개발 기록(web monitoring system dev note)

#### 추천 에디터(Recomand editor) : [microsoft vscode](https://code.visualstudio.com/)

## 기술스택

- 사용예정 기술 스택

Back-End(Source) | Back-End(DB) | Back-End(Server) | Front-End(Client)
--- | --- | --- | ---
. | . | . | bootstrap
. | . | GraphQL | react
. | . | flask | node.js
. | postgresql | python | javascript
python or C | alpine | ubuntu | ubuntu
local | docker | docker | docker

## 개발환경 구성

1. 프로젝트 폴더 생성(make project directory)

```shell
mkdir monitoring
```

1. 도커 개발환경 세팅 (docker development environment setting)

- directory structure

```txt
/monitoring
    /server
    /client
    /db
    .env
    Dockerfile-s
    Dockerfile-c
    docker-compose.yml
```

- .env

```conf
FMON_C_80=[external port]
FMON_S_80=[external port]
FMON_DATA_SERVER=./server
FMON_DATA_CLIENT=./client
FMON_DATA_DB=./db
```

- Dockerfile-s

```Dockerfile
FROM ubuntu:18.04

# install packages
RUN rm -rf /var/lib/apt/lists/*
RUN apt-get update -y
RUN apt-get install -y libpq-dev build-essential
RUN apt-get install -y wget curl git openssl bzip2 patch
RUN apt-get install -y postgresql-client mysql-client mongodb-clients
RUN apt-get install -y python3 python3-pip

# install flask & graphql
RUN pip3 install virtualenv
RUN mkdir /server
RUN cd /server
RUN virtualenv venv
RUN /bin/bash -c "source venv/bin/activate"
RUN pip3 install flask flask-graphql flask-migrate flask-sqlalchemy graphene graphene-sqlalchemy
````

- Dockerfile-c

```Docker
FROM ubuntu:18.04

# install packages
RUN rm -rf /var/lib/apt/lists/*
RUN apt-get update -y
RUN apt-get install -y libpq-dev build-essential
RUN apt-get install -y wget curl git openssl bzip2 patch
RUN apt-get install -y postgresql-client mysql-client mongodb-clients

# install node:12
RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -
RUN apt-get install -y nodejs
```

- docker-compose.yml

```yml
version: "3.7"

services:
    client:
        build:
            context: ./
            dockerfile: Dockerfile-c
        depends_on:
            - server
        ports:
            - ${FMON_C_80}:80
        networks:
            - net
        volumes:
            - ${FMON_DATA_CLIENT}:/client

    server:
        build:
            context: ./
            dockerfile: Dockerfile-s
        depends_on:
            - db
        ports:
            - ${FMON_S_80}:80
        networks:
            - net
        volumes:
            - ${FMON_DATA_SERVER}:/server

    db:
        image: postgres:12-alpine
        networks:
            - net
        volumes:
            - ${FMON_DATA_DB}:/var/lib/postgresql/data

networks:
    net:
```
