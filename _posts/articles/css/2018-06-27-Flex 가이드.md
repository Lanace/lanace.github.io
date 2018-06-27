---
layout: article
title: "Flex 가이드"
categories: articles
modified: 2016-06-01T16:28:11-04:00
tags: [css, flex]
comments: true
ads: true
---


# Flex 가이드

> 기본적으로 `https://css-tricks.com/snippets/css/a-guide-to-flexbox/` 를 번역한거다.
> 추가로 내가 이해한 내용을 바탕으로 재구성하였다.

## 개요

Flexbox 레이아웃은 레이아웃을 좀더 효과적으로 제공하는것에 초점을 맞추고 있다. 컨테이너에서 item들 간에  `align`과 공간을 나누는것, 심지어 크기를 알 수 없거나 동적인 경우에도 효율적인 레이아웃을 제공하길 원한다. 그래서 `flex` 이다.

> `flex`의 원래 단어 뜻은 신축성, 유연성 이다. 즉, 유연하게 레이아웃을 만들어주겠다는 의지인것같다.

`flex layout`에 대한 기본적인 생각은 item의 가로나 세로의 크기가 달라질때 빈 공간들을 채울 수 있는 능력을 갖춘 컨테이너를 제공하는것이다. (대부분 모든 크기의 디스플레이나 스크린 크기를 수용할 수 있도록 하는것이다.) 
`flex container`는 빈공간을 확장하여 채울 수 있고, overflow를 막기위해 줄어들 수 있다.

Most importantly, the flexbox layout is direction-agnostic as opposed to the regular layouts (block which is vertically-based and inline which is horizontally-based). While those work well for pages, they lack flexibility (no pun intended) to support large or complex applications (especially when it comes to orientation changing, resizing, stretching, shrinking, etc.).

가장 중요한것은 `flexbox layout`은 방향에 상관이 없다.

(특히 방향이 바뀌거나, 크기가 바뀌거나, 늘어나거나, 줄어들 때 유용하다.)

> Note: `Flexbox layout`은 Application의 컨포넌트나 작은 규모의 레이아웃과 닮았다. 반면, `Grid layout`은 큰 큐모의 레이아웃을 만드는것을 목적으로 만들었다.

## 배경지식

Since flexbox is a whole module and not a single property, it involves a lot of things including its whole set of properties. 
Some of them are meant to be set on the container (parent element, known as "flex container") whereas the others are meant to be set on the children (said "flex items").

`flexbox`가 전체 모듈이고, 단일 속성이 아니기 때문에, 

만약 `regular layout`이 block과 inline flow directions을 기반으로 하고있으면, `flex layout`인 `flex-flow directions`에 기반하게 된다. 아래 그림을 통해 `flex layout`의 개념을 살펴보자.

## 
