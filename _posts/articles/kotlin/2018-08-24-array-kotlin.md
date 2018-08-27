---
layout: article
title: "간단한 Array 사용 방법 in Kotlin"
categories: articles
modified: 2016-08-24
tags: [kotlin]
comments: true
ads: true
image:
  feature: kotlin_banner2.png
  teaser: kotlin_banner1.jpeg
  thumb: 
---

## -


## Java

``` java
Step[] fragmentArray = {
    new SignupStep1Fragment(),
    new SignupStep2Fragment(),
    new SignupStep3Fragment(),
    new SignupStep4Fragment()
}
```

## Kotlin

``` kotlin
var fragmentArray: Array<Step> = arrayOf(
    SignupStep1Fragment(),
    SignupStep2Fragment(),
    SignupStep3Fragment(),
    SignupStep4Fragment()
)
```