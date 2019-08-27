---
title: "오프라인 환경에서 도커컴포즈 사용(using docker-compose up in offline)"
date: 2019-05-03T11:12:00+09:00
categories:
  - IT
tags:
  - tips
  - docker
  - docker-compose
---

- Tested Environment : Linux(Ubuntu, RHEL)

### 온라인 환경에서 이미지 파일 저장 (online env)
```sh
docker save image-name > image-name.tar
```

### 오프라인 환경 이미지 파일 로드 (offline env)
```sh
docker load < image-name.tar
```

### 이미지 ID확인 (offline env)
```sh
docker images
```
<pre>
REPOSITORY     TAG             IMAGE ID           CREATED           SIZE
image-name     1.1.0             17aadaadaa11     1 hours ago       1.7GB
</pre>

### 도커 컴포저 파일 수정 (offline env)
```sh
vi docker-compose.yml 
```
<pre>
...
image: 17aadaadaa11
...
</pre>
```sh
docker-compose up -d
```