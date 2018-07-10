---
layout: article
title: "Android Splash 화면을 구현하는 올바른 방법"
categories: articles
modified: 2016-07-10T16:28:11-04:00
tags: [android]
comments: true
ads: true
image:
  feature: 
  teaser: 
  thumb: 
---

## 시작하면서

[여기](https://www.bignerdranch.com/blog/splash-screens-the-right-way/) 에 있는 글을 보고 정말 좋은 방법이라고 생각해서 번역하고 살짝 수정해서 글을 썼다.
원본이 더 잘 나와있으니 영어가 자신있으신 분들은 [여기](https://www.bignerdranch.com/blog/splash-screens-the-right-way/) 를 참고하시는게 더 좋을것같습니다ㅠ

## 서론

앱을 구현하다보면 자연스럽게 앱이 준비되는동안 사용자에게 보여줄 Splash화면을 추가해야겠다고 생각하게 된다. 이미 많은 앱에서 Splash 화면을 보여주고 있다.
Splash 화면을 구현하는데는 여러가지 방법이 있겠으나 사실 Splash화면을 사용하는 이유부터 알고 구현을 해야한다.
그런 의미에서 아래에서 설명할 방법이 개인적으로 가장 잘 구현했다고 생각한다....

### Splash screen 이란?

> `Splash screen`은 이미지나 로고, 현재 버전의 소프트웨어를 포함한 그래픽 요소를 보여주는 화면으로, 보통 게임이나 프로그램이 실행되고 있을 때 나오는 화면

### splash screen의 목표

> `Splash screens`은 프로그램이 로딩되고있다는걸 알려주기위해서 사용.
> 
> 뒷단에서 무언가 작업중이다라는걸 알려주기 위함.

안드로이드 앱중 종종 splash 화면이 없는 경우도 있고, 올바르지 못하게 만드는 경우가 매우 많다. 앱 개발자들이 그런 splash화면을 구현하는데 걸리는 시간은 정말 작다.

## Splash 가이드

가이드에 나와있는 splash화면을 읽어보면 놀랄지도 모른다. [Material Design](https://material.io/guidelines/patterns/launch-screens.html) 스팩을 확인해보자.

항상 그런경우는 아니지만, 구글은 splash화면을 별로 좋아하는것같지 않다. 
심지어 이걸 안티페턴이라고까지 부르기도 한다... (~~안티페턴이라고 생각하지는 않는데 뭐 그렇다고 한적이 있다고 한다...~~)

아래 이유때문인데 각각 보면 뭐 이해가 안가는건 아니다.

  * 처음부터 가입을 요구하지 마라
  * splash화면을 사용하지 마라
  * 유명한 서비스와 통합하지 마라
  * 앱의 가치를 높여라

## Splash 구현된 화면

사실 구글이 저렇게 왜 말했는지 조금 모순되는 부분이 있다.
(혹시라도 아직까지 splash화면이 사용자에게서 시간을 뺏는다고 생각하는 사람은 없길 바란다...ㅠ Splash화면은 분명 목적도 있고, 필요성도 명확하다.)

하지만 안드로이드 앱은 시작하는데 약간의 시간을 소요된다. (~~예외적으로 바로 메인에 도달하는 경우도 종종 있긴 하지만;;~~)
어쨋든 피할 수 없는 딜레이가 존재한다. 이러한 시간동안 아무 것도 없는 화면에서 떠나는것 대신에 좀더 좋은것들을 보여줄 생각을 하는게 좋지않은가??
이것이 Splash의 목적이다. 사용자의 시간을 낭비시키지 말고, 흰 화면으로 보여주지 말고, 처음 보여주는 화면을 잘 꾸며보는것...

최근 업데이트된 구글 앱을 본다면 splash화면을 볼 수 있을것이다.
예를들면 유튜브 같이...

![youtube_splash.gif](https://4.bp.blogspot.com/-Rz-5G90sR4M/WKCHNl10-rI/AAAAAAAALf8/PvjKHvH3xW8bTEPn1Y-DfWJbL1hqeSacACLcB/s320/youtube_splash.gif)

저렇게 잠깐 뜨는 화면을 Splash화면이라고 한다.

## Splash 구현 방법

저렇게 올바르게 Splash화면을 구현하는 방법은 생각했던거랑은 조금 다르다.
지금까지 봐왔던 Splash화면은 inflate를 통해 레이아웃 파일을 변경하고, 몇초 후에 Main화면으로 넘기는 방식이였을 것이다.
올바른 방법은 레이아웃 파일을 사용하지 않고, 대신 Activity의 테마 배경을 변경하는 방식으로 구현을 한다. 이렇게 하면 먼저 `drawable`에 xml하나를 만든다.

위의 코드에서 배경색과 이미지를 설정한다.
다음으로, `Splash Activity`의 배경에 테마를 설정한다.
아마 `styles.xml`에 보면 있을거다.

<script src="https://gist.github.com/Lanace/54a653773487659ef478129d68f1ea87.js"></script>

여기에서 `android:windowBackground` 을 방금 만든 splash.xml로 변경한다.
그리고 android manifist 에서 아래와 같이 SplashActivity를 최초 실행되도록 하고, 스타일을 splash로 하면 된다.

<script src="https://gist.github.com/Lanace/3b3c140da23e19b3d5359b0a03cf86b5.js"></script>

마지막으로 `SplashActivity`에서 onCreate부분에 intent를 통해 main으로 이동시켜주면 끝!

<script src="https://gist.github.com/Lanace/fd166772d4003e691aa2dfc6f086f895.js"></script>

자세한 소스는 [여기](http://xn--%20-504mp12c9qbc9qkwj76f/) 에서 확인할 수 있다.

따로 `SplashActivity`에 대해서 처리하지 않아도 된다.
View는 `theme`를 통해 표현될거기 때문이다. UI화면이 준비되었을 때, 바로 넘어가질것이다.

만약에 `SplashActivity`에 레이아웃 파일을 넣어버린다면 이미 앱을 사용자에게 보여줄 준비가 완벽하게 된 이후에 보여주게 되는거다. 따라서 Splash의 의미가 없는것이다.
Splash화면은 앱이 초기화 되기 이전에 아주 잠깐만 뜨기 위한 화면이니 이렇게 하는것이 가장 좋은 방법이라고 생각한다.
