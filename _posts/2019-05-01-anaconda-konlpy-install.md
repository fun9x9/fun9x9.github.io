---
title: "anaconda환경에서 konlpy 설치 (how to install konlpy on anaconda)"
date: 2019-05-01T15:47:00+09:00
categories:
  - IT
tags:
  - tips
  - anaconda
  - python
  - konlpy
  - windows
---

- Tested Environment : 
  - Windows10
  - conda 4.6.14
  - python 3.7

### 관리자권한으로 conda prompt실행
```sh
conda install -c conda-forge jpype1
```

### 관리자권한으로 명령 프롬프트 실행
```sh
pip install konlpy
```

### [자바(JVM)설치 필요](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
- 설치 후 JAVA_HOME 및 Path 설정 필요 