---
layout: article
title: "Context의 종류와 활용법"
categories: articles
modified: 2018-08-14
tags: [android, context]
comments: true
ads: true
image:
  feature: android_common.jpg
  teaser: android_common.jpg
  thumb: android_common.jpg
---

> Android를 개발하면서 가장 많이 본것중에 하나가 `Context` 인데 `Context`가 뭐냐고 물어보면 정확히 정의를 내리지 못하겠다.
> 추상적으로 리소스를 가져오거나, 기기의 정보를 가져올때 사용하는것 이라고 할 수 있겠지만 정확한 정의는 아니다.
> 그래서 이번기회에 한번 정리해보기로 했다.

## Context의 정의

[공식문서](https://developer.android.com/reference/android/content/Context) 에서는 다음과 같이 정의하고 있다.

> Interface to global information about an application environment. 
> This is an abstract class whose implementation is provided by the Android system. 
> It allows access to application-specific resources and classes, as well as up-calls for application-level operations such as launching activities, broadcasting and receiving intents, etc.

* Application 환경에 대한 전역 정보 인터페이스
* Android 시스템이 제공하는 implementation의 추상클레스
* 특정 Application 리소스와 Class에 접근할 수 있도록 하고, Activity나 Broadcasting, Intent를 받는 등의 기능을 수행

이라고 한다. 

## Context가 무엇인가

이름 그대로를 정의하자면 흐름으로, Application이나 Object의 현재 상태를 나타낸다.
새롭게 생성된 object들은 어떤 상황에 있는지를 이해할 수 있도록 한다.
전형적으로, 프로그램의 다른 정보들에 관련된 정보를 얻을 수 있다.

또한, `Context`는 시스템을 다룬다. 데이터베이스나 Preference, 리소스등에 접근할 수 있도록 한다.
안드로이드 앱은 Activity들을 가지고 있는데, 현재 실행중인 애플리케이션의 환경을 다룰 수 있도록한다.
Activity object는 Context object를 상속받는다. 이 context는 안드로이드 환경에 관련된 Resource와 class, 그리고 정보들에 접근할 수 있도록 한다.

`Context`는 거의 어디에나 있고, Android를 개발함에 있어서 매우 중요하다. 그래서 반드시 Context에 대해 정확히 이해할 필요가 있다.
잘못된 `Context`의 사용은 메모리 누수를 발생시킬 가능성을 키운다.

안드로이드에 몇가지 다른 종류의 context가 존재하는데, 어떤것들이 있고 언제 어떻게 사용해야하는지 살펴보자.

## Apllication Context

Application Context는 싱글톤이고, activity에서 `getApplicationContext()`로 접근할 수 있다. 
이 context는 application의 생명주기와 연관이 있다. 
Application context는 현재 context로부터 분리되어있거나, activity 범위를 넘어서 존재할때 사용될 수 있다.

예를들어, 만약에 싱글톤으로 objacet를 생성하고 context를 필요로 한다면, Application context를 전달한다.
이떄 만약 activity의 context를 전달하면 activity에 context를 계속해서 참조하고 있으므로 GC되지 않아서 메모리 누수가 발생할 수 있다.
이러한 경우, activity에서 초기화를 할때 항상 application의 context를 전달한다.
context가 처리해야할것보다 길게 살아있어야 하는걸 알았을 때만 `getApplicationContext()`를 사용하자.

## Activity Context

이 `Context`는 activity에서 사용할 수 있다. 이 `Context`는 activity의 생명주기와 연관이 있다. activity context는 반드시 activity 범위에서 주고받거나, 현재 context에 붙어있는 생명주기의 context일때 사용해야만 한다.

예를들어, 만약 생명주기가 Activity에 붙어있는 object를 생성한다면 activity context를 사용해도 된다.

## ContentProvider에 있는 getContext()

이 context는 Application의 context이고, application context와 유사하게 사용될 수 있다.
`getContext()`를 통해서 사용할 수 있다.

## getApplicationContext()를 사용하면 안되는 경우

* Activity에서 서포팅하는 모든것은 완벽하지 않다. 대부분 GUI와 연관된 작업들을 할때 `getApplicationContext()`를 사용하면 정상적으로 동작하지 않을 가능성이 크다.

* It can create memory leaks if the Context from `getApplicationContext()` holds onto something created by your calls on it that you don’t clean up. 
* With an Activity, if it holds onto something, once the Activity gets garbage collected, everything else flushes out too. The Application object remains for the lifetime of your process.

* 만약 `getApplicationContext()`로부터 clean up 하지 않은 호출로 어떤걸 생성하면 메모리 누수가 발생할 수 있다.
* Activity에서 만약 GC가 어떤걸 잡고있다면, 
* Application object는 프로세스 생명주기에 남아있는다.

## The Rule of Thumb

대부분의 경우, context를 직접적으로 사용하는것은 현재 동작하고 있는곳 내부에서 호출하는것이 좋다.
컨포넌트의 생명주기를 넘지 않는 선에서 참조하는것이 안전하다.
Activity나 Service에 상관없이 Object에 Context를 저장시켜야 할 경우가 있다. 이럴땐 apllication context를 사용하자.


## 참고

http://arabiannight.tistory.com/entry/272: OS에 Process 관점에서의 Context를 바라보고, 왜 Context가 이런 형태를 띄게 되었는지에 대한 설명 
https://medium.freecodecamp.org/mastering-android-context-7055c8478a22: 갓갓.... 그는 도덕책...
