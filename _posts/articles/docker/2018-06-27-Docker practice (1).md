---
layout: article
title: "Docker 실습 -1"
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

# Docker 사용해보기

## Install

기본적으로 리눅스 기반으로 되어있으므로 리눅스에서 진행한다.

> windows나 Mac 에서는 각각 Docker for Mac / Docker for Windows 가 있으므로 다운받아서 진행하도록 하자. 어떤 문제가 있어서 사용하지 못한다면 VM을 돌리는 방법도 있다.

(저는 ubuntu 16.04에서 진행하였습니다.)

```
curl -fsSL https://get.docker.com/ | sudo sh
```

위의 명령어를 입력하여 Docker를 설치하고 아래 명령어를 입력하여 재대로 설치되었는지 확인한다.

```
docker version
```

아마 다음과 같은 화면을 볼 수 있을것이다.

[Docker Image]

위의 이미지에서 알 수 있드시 Client와 Server의 정보가 각각 나오는 것을 알 수 있다. Docker는 하나의 실행파일이지만 실제로 서버와 클라이언트 역할을 할 수 있다.

## 컨테이너 실행하기

### 컨테이너 띄우기

일단 컨테이너를 생성하는 명령어부터 살펴보자.

```
docker run ubuntu:16.04
```

위의 명령어를 입력하면 컨테이너를 생성하고, 시작한다.
`run` 명령어가 입력되었을 때 사용할 이미지가 저장되어있는지 확인한 뒤, 없으면 다운로드한다.

결과적으로 처음 실행한 위의 명령어는 이미지가 없으므로 다운로드를 진행하고, 컨테이너를 실행한다. 하지만 다른 명령어를 더해준게 아니므로 할일이 없어 바로 종료된다. 

> 컨테이너는 프로세스이기 때문에 실행중인 프로세스가 없으면 컨테이너가 종료된다.

### 컨테이너에서 bash 실행하기

이번엔 `/bin/bash` 명령어를 입력해서 `ubuntu:16.04` 컨테이너를 실행해보자.

```
docker run --rm -it ubuntu:16.04 /bin/bash
```

옵션을 `rm`과 `it`를 넣었는데 각각 다음을 위한것이다.
`it`: 키보드 입력을 위한 옵션으로, -`i` 와 `-t`를 동시에 사용한것. 터미널 입력을 위한 옵션이다. 
`rm`: 추가적인 프로세스가 종료되면 컨테이너가 자동으로 삭제.

위의 명령어를 입력하여 가상의 ubuntu를 띄우고, bash를 실행시켰다.
이제 `exit`를 입력하여 나오면 가상의 bash는 종료되고, 컨테이너도 `rm`옵션으로 인해 종료된다. 

### Redis 컨테이너 띄우기

이번엔 Redis 컨테이너를 띄워보자.

```
docker run -d -p 1234:6379 redis
```

위의 명령어에서는 `d`와 `p`옵션을 추가해보았다.
`d`: 백그라운드로 실행한다.
`p`: 컨테이너의 포트를 호스트의 포트로 연결

위의 명령어를 실행하면 컨테이너의 ID를 출력하고 다시 bash로 돌아오는데 백그라운드로 redis가 살아있는것이며 

아래와 같이 실행하면 redis가 살아있음을 느낄 수 있다.

[redis 이미지]

이렇게 백그라운드로 실행되면 실행중인 컨테이너에 접근은 어떻게 하나 궁금할 수 있는데 이런 제어를 위해서 컨테이너마다 ID가 존재한다. 이는 아래에서 다루기로 하자.



### MySQL 5.7 컨테이너 띄우기

3번째로 띄울건 `MySQL`이다.
아래 명령어를 입력해보자.

```
docker run -d -p 3306:3306 \
    -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
    --name mysql \
    mysql:5.7
```

이번에도 `-e`, `--name` 옵션에 대해서 살펴보도록 하자
`-e`: 환경변수를 설정
`--name`: 컨테이너에 읽기 쉬운 ID를 지정

위에서 사용한 `MYSQL_ALLOW_EMPTY_PASSWORD` 옵션은 root 계정에 password 없이 만드는 옵션인데 

> `--name`을 생략하면 자동으로 ID가 만들어진다. 이름은 보통 유명한 과학자나 수식어를 조합해서 랜덤으로 생성한다고 합니다...

### WordPress 컨테이너 띄우기

다음으로는 `WordPress`를 띄워보도록 하자.
`WordPress`는 데이터베이스가 필요하므로 함께 설치해야한다.

3번째로 컨테이너를 띄웠던 mysql을 사용할껀데 `--link`옵션을 사용하여 연결하게 된다. 컨테이너를 띄우기 전에 `--link`옵션을 살펴보고 가도록 하자.

`--link`는 환경변수와 IP정보를 공유하는데 링크한 컨테이너의 IP정보를 `/etc/hosts`에 자동으로 입력하므로 워드프레스 컨테이너가 mysql 데이터베이스의 정보를 알 수 있게된다. (~~무슨 얘기인지 모르겠으니 일단 실습해보자~~)

우선 워드프레스가 사용할 데이터베이스를 생성한다. 

```
mysql -h 127.0.0.1 -u root
create database wp CHARACTER SET utf8;
grant all privileges on wp.* to wp@'%' identified by 'wp';
flush privileges;

quit
```

위의 명령어를 순서대로 입력하면 wp 데이터베이스가 생성되고, 권한도 부여가 된다. 
그럼 docker를 이용해 wp 컨테이너를 만들어보자

```
docker run -d -p 8080:80 \
    --link mysql:mysql
    -e WORDPRESS_DB_HOST=mysql
    -e WORDPRESS_DB_NAME=wp
    -e WORDPRESS_DB_USER=wp
    -e WORDPRESS_DB_PASSWORD=wp
    wordpress
```

위 명령어를 입력한 뒤 웹 브라우저로 들어가 확인해보자.
[127.0.0.1:8080](127.0.0.1:8080) 으로 들어가면 볼 수 있다.

[wordpress 이미지]

잘 실행되고 있는것을 확인할 수 있다.

> 위 예제는 테스트용으로 실제 사용하려면 여러가지 설정을 더 해야하니, 사용하실 분은 다른 글을 참고해주세요~

> `--link` 옵션은 deprecated 되서 사용하면 안되고 `Docker network`기능을 사용해야하지만 쉬운 이해를 위해서 이렇게 했다고 합니다...^^

### Tenserflow 띄우기

마지막으로 `Tenserflow`를 띄워보자
`Tenserflow`는 python으로 되어있고, 관련 패키지까지 설치되어있어야 한다.
따라서 `numpy`, `scipy`, `pandas`, `jupyter`, `scikit-learn`, `gensim`, `BeautifulSoup4`, `Tensorflow` 가 설치되어있는 컨테이너를 만들어보자.

```
docker run -d -p 8888:8888 -p 6006:6006 teamlab/pydata-tensorflow:0.1
```



## 참고 링크

> `https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html#%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-%EC%8B%A4%ED%96%89%ED%95%98%EA%B8%B0` 이 글을 참고하여 직접 해보고 작성한 글이다. 