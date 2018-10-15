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

## `Context`가 무엇인가

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

* It’s not a complete Context, supporting everything that Activity does. Various things you will try to do with this Context will fail, mostly related to the GUI.

* 대부분 GUI와 연관되어져 있다. Activity가 하는것들이 완성되지 않은 context인 경우. 

* It can create memory leaks if the Context from getApplicationContext() holds onto something created by your calls on it that you don’t clean up. With an Activity, if it holds onto something, once the Activity gets garbage collected, everything else flushes out too. The Application object remains for the lifetime of your process.

* 만약 `getApplicationContext()`로부터 


## The Rule of Thumb

In most cases, use the Context directly available to you from the enclosing component you’re working within. You can safely hold a reference to it as long as that reference does not extend beyond the lifecycle of that component. As soon as you need to save a reference to a Context from an object that lives beyond your Activity or Service, even temporarily, switch that reference you save over to the application context.

대부분의 경우, context를 직접적으로 사용하는것은 현재 동작하고 있는곳 내부에서 호출하는것이 좋다.
컨포넌트의 생명주기를 넘지 않는 선에서 참조될 수 있다.
곧 
