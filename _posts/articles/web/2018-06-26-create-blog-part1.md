---
layout: article
title: "블로그 구축기 part1 - aws 설정"
categories: articles
modified: 2016-07-23
tags: [web, SPA, MPA]
comments: true
ads: true
image:
  feature: 
  teaser: 
  thumb: 
---

# 블로그 구축기

## 블로그 구축기 part1

// TODO: AWS 선택한 이유 작성하자

AWS에 대한 자세한 내용은 자습서를 참고하자

### EC2 구입 및 설정

나는 예약 인스턴스를 사용하였다.
장기적으로 이용할것이기 때문에 이게 더 싸고 해보았음

#### Ubuntu 설정

1. 계정 생성


### 도메인 연결

<https://aws.amazon.com/ko/getting-started/tutorials/get-a-domain/?trk=gs_card>
참고 하였음

### AWS codedeploy



### 문제 생겼던 것들

`npm install` 중 설치가 완료되지 않고 killed 되었다는 메세지를 받는 경우.
AWS EC2에서 가장 작은 namo 버전을 사용중인데, namo는 메모리가 0.5G이다. 처음엔 문제가 없겠지 생각했지만 npm install부터 문제가 생겼다.

내 `package.json`은 아래와 같았다.

```jeon
{
  "name": "lanace.io",
  "version": "1.0.0",
  "author": "JeonIlju <lanace93@gmail.com>"
  "dependencies": {
    "axios": "^0.17.1",
    "bootstrap": "^4.0.0-beta.3",
    "bootstrap-vue": "^1.4.1",
    "moment": "^2.20.1",
    "nodemailer": "^4.4.1",
    "vue": "^2.5.13",
    "vue-markdown": "^2.2.4",
    "vue-router": "^3.0.1",
    "vue-toasted": "^1.1.24",
    "vue-youtube-embed": "^2.1.3",
    "vuex": "^3.0.1"
  },
  "devDependencies": {
    "babel-core": "^6.22.1",
    "css-loader": "^0.28.8",
    "eslint": "^3.19.0",
    "karma": "^1.4.1",
    "lodash": "^4.17.4",
    "markdown-loader": "^2.0.2",
    "uglifyjs-webpack-plugin": "^1.1.1",
    "url-loader": "^0.5.8",
    "vue-loader": "^13.3.0",
    "webpack": "^3.10.0",
  }
}

```

~~당연히 적당히 줄인거긴 하지만 꽤 많은 편인것같다.~~

위의 `package.json`을 install하면 `killed`를 받았는데 이 원인은 `kern.log` 파일을 확인해보면 알 수 있다.

``` text
Jan 23 19:05:18 server1 kernel: [135502.129029] Out of memory: Kill process 16877 (npm) score 627 or sacrifice child
Jan 23 19:05:18 server1 kernel: [135502.129095] Killed process 16877 (npm) total-vm:1484264kB, anon-rss:313692kB, file-rss:0kB
```

바로 out of memory 인 것이다.

해결책은 생각보다 간단하다. 서버의 메모리를 높이는것이다.
다른 해결책으로는 swap 파일을 추가하는건데 다음과 같은 commend를 사용해 swap파일을 생성하면 된다.

```bash
sudo fallocate -l 1G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo "/swapfile   none    swap    sw    0   0" >> /etc/fstab
```

그렇게 하면 잘 동작할 것이다.

참고: https://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_%EC%8A%A4%EC%99%91%EA%B3%B5%EA%B0%84_%EC%B6%94%EA%B0%80%ED%95%98%EA%B8%B0_%EC%8B%A4%EC%8A%B5
