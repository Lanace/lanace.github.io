---
layout: article
title: "Docker 개요"
categories: articles
modified: 2016-06-01T16:28:11-04:00
tags: [docker]
comments: true
ads: true
image:
  feature: docker_cover2.png
  teaser: docker_cover.png
  thumb: docker_cover.png
---

# Docker

> Docker란 리눅스를 기반으로

## Docker를 이용한 애플리케이션의 컨테이너화

애플리케이션 컨테이너화의 장단점

컨테이너화가 되지 않은

1. 호스트 컴퓨터에서 직접 구동하는 애플리케이션
1. 

### 컨테이너의 장점

1. 유연성, 리소스 사용 효율성
    애플리케이션 구동에 필요한 파일을 모두 컨테이너에 넣을 수 있음

* 파일 시스템
    별도의 파일시스템 보유
    호스트의 파일시스템 조회 불가 (단, 일부 파일은 마운트 됨)
* 프로세스 테이블
    별도의 프로세스 테이블 보유

* 네트워크 인터페이스

* IPC 기능
    컨테이너마다 독립적인 IPC 기능을 갖고있음
    호스트에서 제공하는 IPC를 사용할 수 없음

* 장치
    컨테이너 안에 있는 프로세스는 호스트의 장치를 볼 수 없음 (단, 권한을 주면 볼 수 있음)

#### 서비스를 컨테이너로 만들어서 실행할 때 이점

호스트에서 직접 구동하는것보다 컨테이너에 담아서 구동하는 이점에는 다음과 같은것들이 있다.

1. 설정
    서비스에 필요한 실행파일, 라이브러리, 설정파일 등의 요소를 담을 수 있음
    따라서 호스트에서 큰 신경쓰지 않아도 됨
2. 격리
    컨테이너마다 파일시스템과 네트워크 인터페이스를 갖추고있어 동시에 여러개의 컨테이너 실행이 가능


### 컨테이너화의 제약사항

...

도커 컨테이너는 패키지를 이미지화 시켜서 만듬
아직 이미지가 안전한지를 완벽하게 보장하지 못함

컨테이너가 다른 컨테이너를 볼 수 없음

## 도커 구성 요소

도커 프로젝트
> 애플리케이션 개발 및 배포를 간결하게 만드는것을 목표로 함

도커 허브 레지스트리
> 개인/기관이 자체적으로 제작한 이미지를 저장, 개발하는 공간. `https://hun.docker.com`

도커 이미지와 컨테이너
> 도커 이미지: 컨테이너에서 애플리케이션을 구동하기 위한것들을 묶는 정적인 단위
> 컨테이너: 도커 이미지를 구동한 인스턴스
서로 명령어가 다르니 주의해서 사용하자!

docker 커멘드
> 이미지와 컨테이너를 직접 다룰땐 docker 커멘드를 사용한다. 
> 

### 레이어 관리

도커에서는 레이어라는 개념을 사용하여 리소스를 중복으로 차지하고 있는것을 막는다.
예를들어 `ubuntu = A + B` 라고 했을때 ubuntu에 enginX가 올라가있다면 `A + B + enginX` 가 될것이다. 즉, 리소스를 공유하고 있다.

### Docker File

도커 파일을 통해서 위의 레이어를 정의하거나 설정값을을 정의할 수 있다.

```
# vertx/vertx3 debian version
FROM subicura/vertx3:3.3.1
MAINTAINER chungsub.kim@purpleworks.co.kr

ADD build/distributions/app-3.3.1.tar /
ADD config.template.json /app-3.3.1/bin/config.json
ADD docker/script/start.sh /usr/local/bin/
RUN ln -s /usr/local/bin/start.sh /start.sh

EXPOSE 8080
EXPOSE 7000

CMD ["start.sh"]
```

## Docker 설치하기

windows에서 동작하기위해선 2가지 방법이 있다.

1. Docker for Windows
1. VM에 Linux를 설치한 뒤 Docker 사용

## Docker 사용해보기

우선 Docker를 실행하기 위한 명령어는 아래와 같음

```
docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```

* 사용 가능한 옵션

### ubuntu 16.04 container 띄우기

간단하게 아래 명령어를 통해서 우분투를 띄울 수 있다.

```
docker run ubuntu:16.04
```

`run` 명령어를 사용하면 사용할 이미지가 있는지 확인하고, 없으면 pull받아서 컨테이너를 생성한 뒤 start한다.

