---
layout: article
title: "Docker 개요 -2"
categories: articles
modified: 2016-06-01T16:28:11-04:00
tags: [docker]
comments: true
ads: true
---

# Docker 실습 (2)

[Docker 실습 (1)]('/Docker 실습 (1)') 에 이어서 작성함

## Docker 기본 명령어

이제 간단한 명령어를 살펴보도록 하자
앞에서 `run` 명령어를 사용해서 컨테이너를 띄웠다. 이렇게 띄운 컨테이너의 상태를 확인하는 명령어는 `ps`이다.

### ps

컨테이너의 목록을 확인하는 명령어이다.

```
docker ps
docker ps -a
docker ps --all
```

아무 옵션도 주지 않으면 현재 실행중인 컨테이너의 목록을 보여준다.
`-a`과 `--all` 옵션을 추가하면 종료된 컨테이너도 확인할 수 있다.
컨테이너가 종료되어도 삭제되지 않고 남아있다.

### stop

실행중인 컨테이너를 중지시키는 명령어는 `stop`이다.

```
docker stop {컨테이너 ID}
```

> Docker의 ID는 64자리 이다. 하지만 전부 입력할 필요 없고, 앞부분이 겹치지 않는 일부만 입력하여도 알아서 잘 찾아간다. (~~똑똑하네...~~)

### rm

컨테이너 제거하는 명령어이다.

```
docker rm {컨테이너 ID}
```

> 중지된 컨테이너를 하나씩 찾아가며 삭제하는건 귀찮으므로 `docker rm -v $(docker ps -a -q -f status=exited)` 명령어를 사용하면 중지된 컨테이너의 id를 가져와 한번에 삭제해준다.

### images

docker가 다운로드한 이미지 목록을 보여준다. 

```
docker images
```

이미지가 많이 쌓이면 그만큼 용량도 많이 차지하므로 적당히 지우는게 좋다. 

#### image 다운로드 - pull

```
docker pull {이름}
```

#### image 삭제 - rmi

```
docker rmi {이름}
```

***

## 컨테이너 내부 명령어

컨테이너 내부에서 사용하는 명령어들이 있다.
`logs`와 `exec`인데 각각을 살펴보자.

### logs

컨테이너의 로그를 볼 수 있다.

```
docker logs {컨테이너 ID}
```

위 명령어를 입력하면 모든 로그가 나오므로 아래와 같이 최근 로그 몇개만 확인하도록 하자

```
docker logs --tail 10 {컨테이너 ID}
```

위의 명령어와 같이 실행하면 실시간으로 로그 조회가 가능하다.
만약 실시간을 멈추고 싶다면 `ctrl` + `c` 를 사용해서 멈출 수 있다.

### exec

컨테이너에 있는 파일을 실행할 때 사용하는 명령어

```
docker exec -it mysql /bin/bash
```

## Update

컨테이너를 업데이트 하려면 새 버전의 이지지를 받고, 기존 컨테이너를 삭제한 뒤, 새 이미지를 run 하면 된다. 하지만 데이터베이스가 컨테이너에 있으면 큰일이니 외부에 데이터베이스를 두는것이 현명하다.

## Compose

복잡한 명령어가 나오기 시작하면 커멘드라인에 입력하기 어려운데 이것을 도와주는것이 Docker Compose이다.

아래 명령어를 통해서 설치 가능하다. 

```
curl -L "https://github.com/docker/compose/releases/download/1.9.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

설치가 완료되면 아래 명령어로 설치된것을 확인하자

```
docker-compose version
```



