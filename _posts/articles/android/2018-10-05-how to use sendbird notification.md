---
layout: article
title: ""
categories: articles
modified: 2018-08-14
tags: [android, fragment]
comments: true
ads: true
image:
  feature: android_common.jpg
  teaser: android_common.jpg
  thumb: android_common.jpg
---

## Push Notifications for Android

Offline 상태에서 메세지가 도착했을 때 Push Notification을 받을 수 있다.
앱이 backgroud상태에 있을때 SDK에서 자동으로 Push Notification을 보낸다.
그러므로 일반적인 경우에 `disconnect()`을 할 필요는 없다.

Push Notification을 받고싶지 않다면 `SendBird.setAutoBackgroundDetection(false)`를 호출한다. 

> Push Notification은 Group Channel만 지원한다. 
> Open Channel은 SDK에서 지원하지 않는다.

앱 사용자에게 Push Notification을 보내기 위한 4단계

* Firebase 대시보드에서 `FCM Server API Key`와 `Sender Id` 생성
* Sendbird 대시보드에 `FCM Server API Key`와 `Sender Id`를 등록
* FCM 클라이언트 코드 추가
* Sendbird SDK에 FCM 등록 token을 등록하고 SendBird FCM message를 파싱

### Step 1: Firebase 대시보드에서 `FCM Server API Key`와 `Sender Id` 생성

이미 `FCM Server API`와 `FCM Sender ID`가 있으면 스킵해도 됨

- Visit the Firebase Console. If do not you have an existing Firebase project for your service, create a new project.
- Click the project card to navigate to the Project Overview page.





### Step 3: Set up FCM client code in your Android app
Please follow Firebase's Set Up a Firebase Cloud Messaging Client App on Android guide on adding configuration code to your Android project to handle FCM messages. The Google FCM sample project can also be a helpful reference.

After completing this step, generate a FCM registration token and have skeleton code to handle FCM messages.



### Step 4: Register FCM Registration Token in the SendBird SDK and parse SendBird FCM messages
Obtain a FCM Registration token from FirebaseInstanceIdService and pass it to the SendBird SDK. onTokenRefresh() in FirebaseInstanceIdService can be the perfect place to do this.
