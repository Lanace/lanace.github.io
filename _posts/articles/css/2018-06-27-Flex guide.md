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




> Flex는 `contatiner`와 `item` 간의 속성이 나뉜다.

### Container 속성

#### display

가장 기본이 되는 속성
`Flex`를 사용하기 위해선 이 속성을 반드시 `flex`로 설정해야한다.
flex를 적용하면 자식들은 모두 flex를 적용받는다. (단, 자식의 자식은 아니다. 직접 연결된 자식만 적용된다.)

``` css
.container {
  display: flex; /* or inline-flex */
}
```

#### flex-direction

`flex-direction`속성은 메인 축을 생성한다. 자식들의 방향을 정의하여, 어떻게 나열할지를 결정한다.
Flexbox는 단방향 레이아웃 개념을 갖고있다. 따라서 flex속성을 받는 자식들은 기본적으로 가로(`rows`) 또는 세로(`columns`)로 배열된다. 

``` css
.container {
  flex-direction: row | row-reverse | column | column-reverse;
}
```

* row (default): 왼쪽에서 오른쪽으로 배열
* row-reverse: 오른쪽에서 왼쪽으로
* column: 위에서 아래로
* column-reverse: 아래서 위로

#### flex-wrap

`flex-wrap`를 사용해서 아이템을 한줄 또는 여러줄에 걸처 표현할 수 있다.

기본값으로, flex의 자식들은 한줄에 딱 맞게 들어가려고 한다.
`flex-wrap` 속성을 사용해서 줄을 바꿔줄 수 있다.

``` css
.container{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```

* nowrap (default): 한줄에 맞추어 배열
* wrap: 아이템 길이가 차면 다음줄로 내려와서 배열 (위에서부터 아래로 채워짐)
* wrap-reverse: 아이템 길이가 차면 다음줄로 내려와서 배열 (아래부터 위로 채워짐)

#### flex-flow (Applies to: parent flex container element)

`flex-flow`속성은 `flex-wrap`와 `‘flex-direction`를 동시에 적을 수 있도록 하는 속성이다.
기본값은 row, nowrap이다.

``` css
.container{
  flex-flow: <‘flex-direction’> || <‘flex-wrap’>
}
```

~~이건... 뭐 그냥 알아만 두면 좋겠다... 크게 중요한 속성은 아닌것같다ㅠ~~


#### justify-content

`justify-content`는 자식들간의 간격을 정의한다. 
모든 자식들이 한줄에 다 표현할 수 없거나, 최대 길이를 찍었을때 남은 공간들을 분배할 수 있도록 한다.
또한, 라인을 넘었을 때도 간격을 조정할 수 있다.

~~말이 어려운데 보면 그렇게 어려운 속성은 아니다. 굉장히 유용한 속성이다.~~

``` css
.container {
  justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
}
```

* flex-start (default): 자식들이 첫줄쪽으로 정렬됨. 라인별로 왼쪽 정렬.
* flex-end: 자식들이 끝줄쪽으로 정렬됨. 라인별로 오른쪽 정렬
* center: 자식들이 가운데 정렬됨
* space-between: 자식들이 고르게 공간을 분배. 첫번째 자식은 왼쪽 끝에, 마지막 자식은 오른쪽 끝에 위치함
* space-around: 자식들이 고르게 공간을 분배. 단, 모든 아이템들이 양쪽으로 동일한 공간을 갖고있기 때문에 공간이 시각적으로 고르게 보이지는 않음. 첫번째 자식은 한칸의 공간만을 갖고있을텐데, 두번째 자식 이후엔 서로간의 공간을 갖고있기 때문에.
* space-evenly: `space-around`의 문제점을 고려한 속성. 모든 자식들이 동일한 공간으로 분배.

#### align-items 

This defines the default behaviour for how flex items are laid out along the cross axis on the current line. Think of it as the justify-content version for the cross-axis (perpendicular to the main-axis).


``` css
.container {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```

* flex-start: cross-start margin edge of the items is placed on the cross-start line
* flex-end: cross-end margin edge of the items is placed on the cross-end line
* center: items are centered in the cross-axis
* baseline: items are aligned such as their baselines align
* stretch (default): stretch to fill the container (still respect min-width/max-width)

#### align-content

This aligns a flex container's lines within when there is extra space in the cross-axis, similar to how justify-content aligns individual items within the main-axis.

Note: this property has no effect when there is only one line of flex items.

``` css
.container {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

* flex-start: lines packed to the start of the container
* flex-end: lines packed to the end of the container
* center: lines packed to the center of the container
* space-between: lines evenly distributed; the first line is at the start of the container while the last one is at the end
* space-around: lines evenly distributed with equal space around each line
* stretch (default): lines stretch to take up the remaining space


### Child 속성

#### order

By default, flex items are laid out in the source order. However, the order property controls the order in which they appear in the flex container.

``` css
.item {
  order: <integer>; /* default is 0 */
}
```