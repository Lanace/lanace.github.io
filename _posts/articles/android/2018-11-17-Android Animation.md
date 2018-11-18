---
layout: article
title: "Android Animation"
categories: articles
modified: 2018-11-17
tags: [android, animation]
comments: true
ads: true
image:
  feature: android_common.jpg
  teaser: android_common.jpg
  thumb: android_common.jpg
---

> [이거 번역중](https://developer.android.com/training/animation/overview) 번역작업

# Animations Overview

애니메이션은 앱에서 무엇이 동작하고 있는지에 대해서 유저에게 알릴 수 있는 시각적 단서를 제공할 수 있다.
특히 UI 상태의 변화나 새로운 컨텐츠가 로드된다거나, 이용가능한 새로운 액션이 추가될 때 유용하다.
애니메이션은 또한 앱을 좀더 세련되게 해주고, 높은 퀄리티와 느낌을 줄 수 있다.

안드로이드는 원하는 애니메이션의 타입에 따라 여러가지 API들을 포함하고 있다. 그래서 이 글에서는 그런 여러 애니메이션에 대해 간단히 정리할 생각이다.

애니메이션에 대해 좀더 자세히 이해하고싶다면 `meterial design` 가이드를 참고해보자.
[Meterial Design](https://material.io/guidelines/motion/material-motion.html)

## 비트맵 애니메이션

`icon`이나 `일러스트`와 같은 비트맵 그래픽을 애니메이션에 사용하고 싶다면, `drawable animation API`를 사용하게 될거다.
보통, 그러한 애니메이션은 drawable 리소스에 static하게 정의되어져있다. 때에 따라서는 runtime에 정의할수도 있다.

예를들어, 버튼을 클릭했을 때 재생버튼이 멈춤 버튼으로 바뀌는건 두가지 액션이 서로 연관되어져 있다는 관점에서 아주 좋은 커뮤니테이션 방법이다.
그리고 멈춤은 다른 시각적인 방법이다.

더 자세한 정보는 `Animate Drawable Graphics` 를 읽어보자

[Animate Drawable Graphics](https://developer.android.com/guide/topics/graphics/drawable-animation)

## Animate UI visibility and motion

레이아웃에 있는 View의 위치가 변하거나 가시성이 변하도록 할 필요가 있을 때, `subtle 애니메이션`이 어떻게 UI가 바뀌는지 이해하는데 도움이 될것이다.

View를 현재 레이아웃에서 이동하거, 보이거나, 숨기기 위해서는 `android.animation` 패키지에서 제공하는 애니메이션 시스템 속성을 사용해야한다.
`android.animation`는 Android 3.0(API level 11) 부터 사용 가능하다.
이 API들은 View object에 일정 시간동안의 속성을 바꿀 수 있다. 지속적으로 view의 속성이 바뀌는대로 view를 다시 그리면서 애니메이션을 그린다.
예를들어, 위치속성이 바뀔때, view는 스크린을 가로질러서 이동한다던가, alpha속성을 변경시킨다면, view는 fadein 되거나 fadeout 될것이다.

To create these animations with the least amount of effort, you can enable animations on your layout so that when you simply change the visibility of a view, an animation applies automatically. For more information, see Auto Animate Layout Updates.

최소한의 노력으로 그러한 애니메이션을 생성하기 위해선, view의 가시성이 간단하게 변한다거나, 애니메이션을 자동으로 적용할 수 있다.
많은 정보를 보려면 [Auto Animate Layout](https://developer.android.com/training/animation/layout.html)를 참고하자.

어떻게 애니메이션 시스템 속성을 통해 애니메이션을 생성하는지 배우려면 애니메이션 속성을 보자.
또는 아래 있는 일반적인 애니메이션 생성 방법을 살펴보자.

[FadeIn과 FadeOut](https://developer.android.com/training/animation/reveal-or-hide-view#Crossfade)

[Visibility류 애니메이션](https://developer.android.com/training/animation/reveal-or-hide-view#Reveal)

[Swap류 애니메이션](https://developer.android.com/training/animation/reveal-or-hide-view#Cardflip)

[Size와 Zoom 관련 애니메이션](https://developer.android.com/training/animation/zoom)

## 물리 기반 모션

Whenever possible, your animations should apply real-world physics so they are natural-looking. For example, they should maintain momentum when their target changes, and make smooth transitions during any changes.

언제든 가능하다면, 애니메이션은 현실세계의 물리법칙 기반의 애니메이션일것이다. 그래서 자연스럽게 보이도록 하고싶을것이다.
예를들어, 타깃이 변할때 가속도를 유지하면서 부드럽게 이동하길 바랄것이다.

To provide these behaviors, the Android Support library includes physics-based animation APIs that rely on the laws of physics to control how your animations occur.

그러한 행동을 제공하기 위해서 `Android Support library`는 물리기반 애니메이션 API를 포함하고 있다. 
이 API는 어떻게 애니메이션을 발생시키고 제어하는지 물리법칙에 기반해 개발되어져있다.

두가지 일반적인 물리기반 애니메이션은 아래와 같다.

`Spring Animation`, `Fling Animation`

Animations not based on physics—such as those built with ObjectAnimator APIs—are fairly static and have a fixed duration. If the target value changes, you need to cancel the animation at the time of target value change, re-configure the animation with a new value as the new start value, and add the new target value. Visually, this process creates an abrupt stop in the animation, and a disjointed movement afterwards, as shown in figure 3.

매니에이션을 물리적인것에 기반해있지는 않는다. 
만약 타깃의 값이 변하면, 

Whereas, animations built by with physics-based animation APIs such as DynamicAnimation are driven by force. The change in the target value results in a change in force. The new force applies on the existing velocity, which makes a continuous transition to the new target. This process results in a more natural-looking animation, as shown in figure 4.




## Activity간에 애니메이션

On Android 5.0 (API level 21) and higher, you can also create animations that transition between your activities. This is based on the same transition framework described above to animate layout changes, but it allows you to create animations between layouts in separate activities.

Android 5.0 (API level 21) 이상부터 Activity사이에 애니메이션을 만들 수 있다. 이것은 
하지만 이건 분리된 Activity들 에서만 사용할 수 있다.

You can apply simple animations such as sliding the new activity in from the side or fading it in, but you can also create animations that transition between shared views in each activity. For example, when the user taps an item to see more information, you can transition into a new activity with an animation that seamlessly grows that item to fill the screen, like the animation shown in figure 5.



As usual, you call startActivity(), but pass it a bundle of options provided by ActivityOptions.makeSceneTransitionAnimation(). This bundle of options may include which views are shared between the activities so the transition framework can connect them during the animation.

For all the details, see Start an Activity with an Animation. And for sample code, check out ActivitySceneTransitionBasic.

