---
layout: article
title: "ubuntu 설치 후 환경설정 할것들"
categories: articles
modified: 2016-06-01T16:28:11-04:00
tags: [linux]
comments: true
ads: true
image:
  feature: ubuntu.jpg
  teaser: ubuntu_desktop.png
  thumb: ubuntu.jpg
---

# ubuntu 설치 후 환경설정 할것들

> 간혹 Linux를 쓸 일이 많은데 설치할때마다 검색하고 하는게 귀찮아서 정리해둔다. 필요하신 분들은 알아서 가져가세요ㅋ 제가 ubuntu에 뭔가 설치할때마다 업데이트 할 예정입니다.

## base

> 기본적으로 필요한 내용들

```
sudo apt-get update
```


## mysql

```
sudo apt-get install mysql-server
```

위 명령어 입력시 설치가 진행되다가 root의 암호 입력하는 부분이 나옴
암호 입력후 잘 기록해두자.

### Mysql 명령어

* 서버 시작

```
service mysql start
```

* 서버 종료

```
service mysql stop
```

