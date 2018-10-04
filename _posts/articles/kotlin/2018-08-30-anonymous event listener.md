---
layout: article
title: "kotlin에서 이벤트를 바인딩 하자"
categories: articles
modified: 2016-08-30
tags: [kotlin]
comments: true
ads: true
image:
  feature: kotlin_banner2.png
  teaser: kotlin_banner1.jpeg
  thumb: 
---

## 서론

Kotlin을 처음 하다보니 기본적인 부분에서도 막힌다. 
이번엔 이벤트를 바인딩 하는 방법에 대해서 정리하자.

## 정의해야 하는 메서드가 하나인경우

일반적으로 버튼에 이벤트를 바인딩 하는 방법으로는 아래 코드와 같은 방법을 많이 사용한다.

```kotlin
signOutButton.setOnClickListener {
  // TODO: something
}
```

`onClickListener`에서는 override 해야하는 메서드가 `onClick` 하나이기 때문에 위와같이 해도된다.

## 정의해야 하는 메서드가 복수인경우

하지만 여러개의 메서드를 정의해야하는 것은 어떻게 해야할까

방법은 두가지 정도가 있다.
하나는 java의 impliment를 받아 사용하는것과 같이 하는 방법과 무명 메서드를 사용하는 방법이다.

```kotlin

```


```kotlin
ageSeekBar.setOnSeekBarChangeListener(object: SeekBar.OnSeekBarChangeListener {
    override fun onProgressChanged(seekBar: SeekBar?, progress: Int, fromUser: Boolean) {
        TODO("not implemented") //To change body of created functions use File | Settings | File Templates.
    }

    override fun onStartTrackingTouch(seekBar: SeekBar?) {
        TODO("not implemented") //To change body of created functions use File | Settings | File Templates.
    }

    override fun onStopTrackingTouch(seekBar: SeekBar?) {
        TODO("not implemented") //To change body of created functions use File | Settings | File Templates.
    }
})
```