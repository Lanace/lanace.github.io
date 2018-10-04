---
layout: article
title: "왜 Fragment는 기본 생성자를 사용해야만 하는가?"
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

## SendBird 사용법

### How SendBird works with messaging

`SendBird`가 메세지를 보내는 방법
메세지를 만드는 방법은 간단하다. 사용자가 연결하고, 체널에 들어가서 체널에 있는 모든 사용자와 메세지를 보내고 받으면 된다.
체널에는 두가지 방법이 있는데 바로 `Open Channel`과 `Group Channel` 이다.

`Open Channel`은 누구나 들어와서 가볍게 메세지를 주고받을 수 있는 체널이다.
`Group Channel`은 기본적으로 비공개 체널이다. 초대를 통해서만 들어올 수 있다. 하지만 Open Channel 처럼 동작하게 만들수도 있다.

#### Open Channel


#### Group Channel


SDK를 사용할때 메세지는 자동으로 `channel event handlers`에 전달된다. 앱의 인스턴스는 SendBird의 서버에 연결될 뿐만아니라, `onMessageReceived()`나 `onUserJoined()`에 여러가지 정보들을 Callback으로 받는다.


### 설치

#### Step 1: 대시보드에 SendBird Application 생성

SendBrid Application은 `User`, `message`, `channel`과 같은 모든 정보들을 구성하고 있다. 
새로운 SendBird Application을 생성하자. 회원가입은 Google이나 GitHub등으로 가입이 가능하다.

한번 회원가입하면, 모든 사용자는 다른사람들과 플렛폼을 뛰어넘어 대화할 수 있다. 
추가적인 설치 없이 `iOS`, `Android`, `Web`을 지원한다.
하지만 모든 데이터는 하나의 Application 범위에 제한된다. 즉, 다른 SendBird Application 사용자와는 대화할 수 없다.

#### Step 2: Sdk 설치

Installing the SDK is simple if you’re familiar with using external libraries or SDK’s in your projects. To install the SendBird SDK using Gradle, add the following lines to build.gradle file at the app level.

만약 프로젝트에 외부 라이브러리나 SKD를 사용하고 있다면 설치하기 쉽다.
SendBird SDK를 설치하기 위해서 아래 코드를 `build.gradle`에 넣는다.

```
repositories {
    maven { url "https://raw.githubusercontent.com/smilefam/SendBird-SDK-Android/master/" }
}
dependencies {
    implementation 'com.sendbird.sdk:sendbird-android-sdk:3.0.75'
}
```

만약에 `.jar` 파일을 다운로드 받고싶다면 [링크](https://sendbird.com)에서 다운받아서 사용할 수 있다.
~~여기서 그 방법은 생략한다...~~

#### Step 3: SDK에 시스템 권한 허가
SendBird SDK는 시스템 권한을 요구하는것이 몇가지 있다. 그러한 권한은 SDK가 SendBird의 서버에 연결하고, 기기의 외부 저장소에 파일을 읽고 쓰게 하는것들을 허락하도록 한다.
아래 나오는 코드를 `AndroidManifest.xml`에 추가한다.

```
<uses-permission android:name="android.permission.INTERNET" />

<!-- READ/WRITE_EXTERNAL_STORAGE permissions are required to upload or download files from/into external storage. ->
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

#### (선택) Step 4: Proguard 설정

`minifyEnabled`을 설정할 때 `proguard-rules`파일에 아래 코드를 추가하면 된다.

```
-dontwarn com.sendbird.android.shadow.**
```

### 사용법

The Android SDK abstracts messaging into a simple and straightforward process. To send your first message, do the following steps:
Android SDK는 간단하고, 직접적인 프로세스에서 메세지를 보낸다. 

> `SendBird.init()` 를 제외하고 나머지 메소드들은 비동기다. 
> 즉, 받는 모든 메세지들은 `Callback`을 통해서 받는다. 
> 중첩된 메소드에서 간단하게 처리하는 방법은 Step4에 있는 체널에 들어갈때 호출된 이후 받는 CallBack을 살펴보자(?)

#### Step 1: SDK 초기화

초기화는 SDK가 Android Context에 바인딩한다. 즉, 연결이나 상태 변화에 응답하는것을 허락하게 한다.
이때 SendBird Application에서 발급받은 `App ID`가 필요하다.

> `SendBird.init()`는 최초 한번만 실행한다.
> 그래서 SDK 초기화는 Application class에 `onCreate()` 에서 호출하는걸 권장한다.

```
SendBird.init(APP_ID, Context context);
```

위의 코드를 앱이 최초 실행되는 곳에서 호출한다.

#### In my case...

Appication class를 만들고 해당 Application의 `onCreate()`에 위의 코드를 삽입하였다.


### Step 2: SendBird Server에 연결

Connect a user to a SendBird server using their User ID. Any untaken user ID creates a new user in the SendBird application before connecting, while an existing ID makes the user log in directly. Authentication with Access Tokens are discussed in the Authentication section.

연결은 사용자의 `User Id`를 사용해서 할 수 있다. 등록된적 없는 `User Id`는 연결되기 전에 SendBird application에 새롭게 등록된다.
Token으로 접근하는것은 [인증 세션]()에서 자세하게 살펴보자.

```
SendBird.connect(USER_ID, new ConnectHandler() {
    @Override
    public void onConnected(User user, SendBirdException e) {
        if (e != null) {    // Error.
            return;
        }
    }
});
```

위의 코드를 삽입하여 서버에 접속한다.

기본적으로, SendBird 서버는 채널에 들어가기 위해서  `User Id`를 사용한다. 접속 요청을 할때, 사용자 DB에서 매칭되는 `User Id`를 찾는데, 만약 존재하지 않는다면 새로운 계정을 생성한다. 
`User Id`는 문자열로 된 이메일이나 UID같은 값을 보통 사용한다.

이런 간단한 인증 절차는 개발하거나, 추가적인 보안을 요구하지 않을때 유용하다.

> 위의 코드를 실행하였을 때 `API-TOKEN is missing error`가 발생하였다면 초기화를 하지 않은것이니 초기화를 한번 하고 요청하길 바란다.

### SendBird Server에 연결 해제

더이상 메세지를 온라인 상태에서 받지 않을때 연결을 해제한다. 연결이 해제되면 Push Notification을 통해 `Group Channel`메세지를 받는다.

연결 해제될때 모든 `SendBird.addChannelHandler()`와 `SendBird.addConnectionHandler()`를 통해 등록된 `event handlers`와 `callbacks`은 제거된다.
또한 캐시된 데이터도 날아간다. 

아래 코드를 통해 연결을 해재할 수 있다.

```
SendBird.disconnect(new SendBird.DisconnectHandler() {
    @Override
    public void onDisconnected() {
        // You are disconnected from SendBird.
    }
});
```

### 프로필 업데이트

`updateCurrentUserInfo()`를 통해서 사용자의 닉네임과 프로필 이미지를 설정할 수 있다.

```
SendBird.connect(USER_ID, new ConnectHandler() {
    @Override
    public void onConnected(User user, SendBirdException e) {
        if (e != null) {    // Error.
            return;
        }

        SendBird.updateCurrentUserInfo(NICKNAME, PROFILE_URL, new UserInfoUpdateHandler() {
            @Override
            public void onUpdated(SendBirdException e) {
                if (e != null) {    // Error.
                    return;
                }
            }
        });
    }
});
```

> 이때 이미지는 이미지의 경로를 통해 지정할 수 있으며, File을 업로드 할 수 있지만 그건 SendBird의 문서를 참고하길 바란다.


### Application

> 준비가 다 끝났으면, `SendBird application`이 채팅 서비스를 올바르게 사용하기 위한 기능적 제한에 대해서 이해할 필요가 있다.
> 또한 사용자 유저가 올바르게 사용할 수 있는 몇가지 방법들을 제공한다.

#### 기본 설정값

몇몇 유저의 비정상적인 활동을 막기위해서 몇가지 기본적인 제한들이 걸려있다.

Imposed on	Limit	If exceeded
User	5 messages per second	Excess messages are not sent to a channel, and not saved in the database.
Channel	5 messages per second	Excess messages are not displayed in a channel, but saved in the database.
The limits above are basic numbers of our premium features, spam flood protection and smart throttling, which could be adjusted in some situations. Currently in the dashboard, paying customers can control the limit of the smart throttling.


### Channal Type

> 내가 필요한건 `Group Channal`이다. 따라서 `Open Channal`은 [공식 문서]()나 다른 글을 참고하길 바란다.

#### Group Type

* Private group channel
`Private group channel`이 기본 설정값이다. 아래 그룹의 설정을 지정하지 않으면 `Private group channel`로 설정된다.

`Group Channel`은 제한된 사람들간의 대화를 제공한다. 
기본적으로, 사용자는 이미 채팅방에 참여하고 있는 사용자의 초대를 통해서 대화에 참여할 수 있다.
초대받은 사용자가 초대를 받아들일지, 받아들이지 않을지 결정할 수 있다.

* Distinct : 
`Distinct group channel`은 항상 . 만약 새로운 사용자가 초대되거나 떠났다면, `Distinct`속성은 자동적으로 disabled 된다.
이 방법을 사용하면 트위터의 DM처럼 `group channel`을 재사용 할 수 있다.

* Public : 
`Public group channel`은 초대 없이 누구나 참여할 수 있다. 유저는 자유롭게 채널에 들어왔다 나갈 수 있다. 하지만 `Private group channel`은 반드시 초대되어야 할 수 있다.

* Ephemeral : 
`Ephemeral group channel`은 메세지가 데이터베이스에 저장되지 않는다. 오래된 메세지는 받을 수 없다.


### Group Channel

위에서 말했드시 `Group Channel`은 제한된 사용자 끼리 대화를 할 수 있도록 한다.
이때 private인지 public인지 결정할 수 있다.

`private group channel`은 이미 채팅방에 있는 사용자의 초대에 의해서만 입장이 가능하다.
1:1 메세지에서 private group으로 만드는게 가능하다.

`public group channel`은 초대 없이 자유롭게 입장이 가능하다. 100명이 기본 설정이고 요청을 통해 추가할 수 있다.



#### group channel의 타입 선택

With our Android SDK, you can use a variety of behaviour related properties when creating different Group Channels. You can create a group channel after configuring these properties.
Android SDK를 통해 다른 그룹채널을 만들때 타입을 선택할 수 있다. 
생성한 이후에 설정을 바꿀수도 있다.

##### Private vs. Public
A Private group channel (default setting) can be accessed only by users that have accepted an invitation by an existing member of that group. On the other hand, a Public group channel can be accessed by any user without an invitation, like an open channel.

##### Ephemeral vs. Persistent
Messages sent in an Ephemeral group channel are not saved in SendBird infrastructure's database. As such, the old messages that scroll up beyond the user's display due to new messages cannot be retrieved. On the other hand, messages sent in a Persistent group channel (default setting) are stored permanently in the database.

##### 1-on-1 vs. 1-on-N
A Distinct group channel can be reused for the same members. If a new member is invited, or if a member leaves the channel, then the Distinct property is disabled automatically. For example, when attempting to create new group channel with 3 members, A, B, and C, if a channel with same members already exists, a reference to the existing channel is just returned to who has attempted new channel.

> Consequently, we recommend turning on the Distinct property in 1-on-1 messaging channels to reuse the existing channel when a user directly sends a message to a friend. 
> If the property is turned off, a new group channel is created with the same friend even if there is a previous chat between them, and you can't see the old messages or data.


#### 추가 정보와 함께 그룹채널 생성

`group channel`은 SDK를 통해 생성될 수 있다.
두명의 `User Id`를 넣어 1:1 대화방을 만든다.

만약에 1:1 채팅을 만들려면 `Distinct` 속성을 true로 한다.

```
GroupChannel.createChannelWithUserIds(USER_IDS, IS_DISTINCT, new GroupChannel.GroupChannelCreateHandler() {
    @Override
    public void onResult(GroupChannel groupChannel, SendBirdException e) {
        if (e != null) {    // Error.
            return;
        }
    }
});
```
