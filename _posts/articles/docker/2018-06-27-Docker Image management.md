---
layout: article
title: "Docker Image 관리"
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

# Docker Image 관리

Docker에서는 수많은 Image를 제공하고 있으며 이 Image만 있으면 실행하는것은 크게 어렵지 않다.
이번엔 Image를 찾고, 가져오고 저장하는 방법을 살펴보자

registry: 이미지를 제공하는 repository들에 대한 정보를 제공하는 장소
repository: 특정 이미지를 제공하는 장소

예를들어 docker.io라는 registry가 있고, docker.io/ubuntu 라는 repository가 있다.

## 이미지 검색

[Docker Hub Registry](https://hib.docker.com)을 통해서 제공하고 있다.

### Command로 검색


아래와 같이 커멘드를 통해서 검색할 수 있다. 

```
docker search ubuntu
docker search centos
docker search fedora
```

커멘드 실행의 결과는 이미지마다 ranking순으로 정렬되어 출력된다.
`search` 커멘드에는 몇가지 옵션을 추가할 수 있는데 다음과 같은것들이 있다.

```
docker search -s 10 fedora
docker search --no-trunc=true mysql
docker search --automated=true centos
```

`-s` 옵션은 별표의 개수가 이 옵션에서 지정한 값보다 위에 있는 이미지만 검색한다.
`--notrunc`는 결과 출력시 description이 ...으로 생략되지 않는다.
`--automated`는 주기적으로 자동빌드하는 이미지만 화면에 보여준다. 

### Docker hub에서 검색

github와 같이 docker 이미지를 모아둔 사이트가 있으니 거기서 검색하여 가져올 수 있다. https://hub.docker.com

### 다른 repository에서 검색

...

## 이미지 받기

`docker pull`을 통해서 찾은 이미지를 가져올 수 있다.

* 레지스트리 이름
* 사용자 이름
* 태그
    하나의 이미지에 여러가지 이름을 붙일 수 있음. 주로 버전명으로 씀


### 옵션

`-a`를 지정하면 모든 종류의 이미지를 가져온다. 

```
docker pull -a ubuntu
```

## 이미지 저장

이미지를 가져올 때 `pull`이 아닌 방식을 사용해서 받아올 수 있다. 바로 저장된 이미지를 통해 받아올 수 있는건데 tar 파일을 사용한다. 

`docker save` 명령어를 사용하면 특정 repository에 이미지를 저장할 수 있다.
이 커멘드에 원하는 repository를 지정하면된다. 

```
docker save -o allcentos.tar centos
du -sh allcentos.tar
scp allcentos.tar host2:/tem
```

위와같이 실행하면 allcentos.tar파일이 host2의 /tmp 디렉터리에 복사된다.
그 다음 host2 시스템에 로그인 한 뒤, 아래 명령어와 같이 allcentos.tar 파일을 불러오면 다음과 같은 결과를 얻을 수 있다.

```
docker load -i /tem/allcentos.tar
docker images | grep centos
```

## 이미지 태깅

태깅을 사용하는 이유는 다음과 같다.

* 버전 번호
    빌드한 이미지에 버전 번호를 붙여 관리
* 버전 이름
    경우에 따라 이미지의 특정 버전에 이름이 붙음
* 사용자 이름
    DOCKER.IO 레지스트리에 사용자 계정을 생성하면 레지스트리에 속하는 이미지를 구분할 수 있도록 태깅한다.
* 레지스트리 이름과 포트
    이미지에 레지스트리 이름을 추가하기 위함
* latest
    태그를 명시하지 않으면 `:latest`라는 태그가 자동으로 붙는다.

### 이미지에 이름 붙이기

1. 이미지 빌드

```
docker build -t fedweb
```

docker file로 이미지를 처음 빌드할 때 이름을 지정한다.
위의 명령어를 사용하면 `fedweb`이라는 이름으로 지정된다.

2. 컨테이너 commit

컨테이너 실행중 변경사항을 반영할 수 있다.

```

```
3. 이미지 export / import

`docker export` 커멘드를 사용하여 이미지를 tar 파일로 묶어서 export한다.
