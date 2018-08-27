---
layout: article
title: "Flex 사용방법과 속성"
categories: articles
modified: 2016-08-23
tags: [css, flex]
comments: true
ads: true
---

# Flex 가이드

## 개요

`Flexbox layout`은 화면 구성을 좀더 효과적으로 제공하는것에 초점을 맞추고 있다. 
Container에서 item들 간에 정렬과 남는 공간을 나누는것, 특히 크기를 알 수 없거나 동적인 경우에도 효율적인 레이아웃을 제공하길 원한다.

**그래서 `flex` 이다.**

> `flex`의 원래 단어 뜻은 신축성, 유연성 이다. 즉, 유연하게 레이아웃을 만들어주겠다는 의지인것같다.

`Flex layout`에 대한 기본적인 생각은 item의 가로나 세로의 크기가 달라질때 빈 공간들을 채울 수 있는 능력을 갖춘 컨테이너를 제공하는것이다. 
(대부분 모든 크기의 디스플레이나 스크린 크기를 수용할 수 있도록 하는것이다.)
`Flex container`는 빈공간을 확장하여 채울 수 있고, overflow를 막기위해 줄어들 수 있다.

가장 중요한건 `Flexbox layout`은 기존에 있는 일반적인 레이아웃이랑은 조금 다른 사상을 가지고있다.
`block`은 수직적 기반을 둔 레이아웃이고, `inline`은 수평적 기반을 둔 레이아웃이다.
그러한것들이 페이지에서 잘 동작하는 반면에 `block`이나 `inline`은 대규모나 복잡한 애플리케이션에 flexibility 하지 못하다.
특히 방향을 바꾸거나, 크기를 조정하거나, 늘리거나 줄이는 등의 수정에는 좀 귀찮고 취약한점이 있다.

이러한 문제를 해결하기 위해 나온것이 flexbox layout에 대한 개념이다.

`flexbox layout`은 방향에 상관이 없다는게 중요하다.
특히 방향이 바뀌거나, 크기가 바뀌거나, 늘어나거나, 줄어들 때 유용하다.

> Note: `Flexbox layout`은 Application의 컨포넌트나 작은 규모의 레이아웃과 닮았다. 반면, `Grid layout`은 큰 큐모의 레이아웃을 만드는것을 목적으로 만들었다.

## 배경지식

`Flexbox`는 여러개의 프로퍼티로 구성된 하나의 모듈이다. 즉 여러가지의 프로퍼티들을 가지고 있다.
몇몇 프로퍼티들은 container에서 사용되는 속성이고, 다른 프로퍼티들은 자식에서 사용되는 속성들이다.
이때 container는 `flex container`라고 부르고, 자식들은 `flex items`라고 부른다.

만약 `regular layout`이 block과 inline flow directions을 기반으로 하고있으면, `flex layout`인 `flex-flow directions`에 기반하게 된다. 아래 그림을 통해 `flex layout`의 개념을 살펴보자.

> Flex는 `contatiner`와 `item` 간의 속성이 나뉜다.
> 모든 속성을 다 외울 필요는 없는것같고, 이런 속성이 있고, 이런 기능이 있구나 정도만 알아두었다가 나중에 필요한 일이 생기면 찾을 수 있을정도는 되야될것같다.
> 그런 속성이 있는지 없는지도 모르면 곤란하니^^;;

![flex container](//css-tricks.com/wp-content/uploads/2014/05/flex-container.svg)
![flex itmes](//css-tricks.com/wp-content/uploads/2014/05/flex-items.svg)

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

![flex direction](//css-tricks.com/wp-content/uploads/2013/04/flex-direction2.svg)

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

![flex wrap](//css-tricks.com/wp-content/uploads/2014/05/flex-wrap.svg)

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

![justify-content](//css-tricks.com/wp-content/uploads/2013/04/justify-content-2.svg)

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

`align-items`은 어떻게 자식들이 현재 한 라인에서 펼쳐질지에 대한 정의를 하는 속성이다.
`justify-content`의 세로축 버전이라고 생각하면 될것같다. 

![align items](//css-tricks.com/wp-content/uploads/2014/05/align-items.svg)

``` css
.container {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```

* flex-start: 라인에 위쪽에 정렬되어 배열
* flex-end: 라인에 아래쪽에 정렬되어 배열
* center: 축에 중심에 위치.
* baseline: 각 자식들간의 baseline을 기준으로 맞춰짐. 
* stretch (default): 부모에 꽉 채워서 늘어남. 단, `min-width`와 `max-width` 속성은 지켜진다.

여기서 주의깊게 봐야할 점은 stretch가 기본 속성이라는것이다. 
개인적인 생각으론 stretch해서 사용하는 경우는 거의 없고, center나 baseline으로 사용하는 경우가 좀더 많았다.

#### align-content

`align-content`는 세로축에 남은 공간이 생겼을때 분배하는 방식을 정의한다. 
여러줄이 있을 때 남은 세로축 공간을 어떻게 나눠가질지 정의한다.

`justify-content`속성이 각각의 자식들에게 남는공간을 어떻게 적용할것인가를 설정한것과 비슷하게 줄별로 적용한다고 생각하면 편할것같다.

단, 이 속성은 한줄에 있는 자식들에게는 영향을 주지 않는다. 여러 줄에 있을때만 영향을 줄 수 있다.

![align content](//css-tricks.com/wp-content/uploads/2013/04/align-content.svg)

``` css
.container {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

* flex-start: 라인들을 위쪽으로 묶음.
* flex-end: 라인들을 아래쪽으로 묶음.
* center: 라인들을 중앙으로 묶음.
* space-between: 라인들간의 간격을 동일하게 분배. 첫줄과 끝줄은 위와 아래로 딱 달라붙게 된다.
* space-around: 라인들간의 간격을 동일하게 분배. 첫줄과 끝줄도 동일하게 간격을 갖는다.
* stretch (default): 남은 공간을 각각의 라인들이 늘어나서 채움.

이 속성도 마찬가지로 stretch가 기본값이다. ~~이건 잘 모르겠다~~

### Child 속성

#### order

기본적으로, flex의 자식들은 dom에 작성된 순서대로 배치된다. 
하지만 `order` 속성을 사용해서 flex container에서 몇번쨰로 위치할지 결정할 수 이싿.

![order](//css-tricks.com/wp-content/uploads/2013/04/order-2.svg)

``` css
.item {
  order: <integer>; /* default is 0 */
}
```

> 근데 이방법은 별로 좋은 방법같지는 않다. 
> 혼동을 줄 수 있기때문이다. 기본적으로 dom에 그려진대로 올라오길 예상할텐데 스타일로 어떤 자식이 첫번째로 배열되는게 일반적이지는 않기때문이다.
> 따라서 적절하게 사용할 필요가 있다. 
> 예를들면... 클릭된 자식이 가장 위에 나올때라던가... 뭐 그런 특정 이벤트 상황? 그럴때 사용하는게 좋을것같다. ~~그렇다... 그냥 내 개인적인 의견이다ㅠ~~

#### flex-grow

`flex-grow`속성은 남는 공간을 자식이 얼마나 가져갈지를 결정하는 속성이다.
한 container에서 3개의 자식이 `flex-grow`속성을 각각 1, 2, 3으로 가지고있다면 남는 공간을 1:2:3 비율로 가져간다.

만약에 모든 자식들이 `flex-grow`가 1로 설정되어있다면, 남는 공간은 모든 자식들에게 공통으로 분배가 될것이다.
다른 예로 하나의 자식만 `flex-grow`가 2로 설정되어있고 다른 자식들은 1로 설정되어있다면, 남은 공간은 2:1로 분배가 될것이다.

즉, 비율로 설정이 되는것이다. 

![flex grow](//css-tricks.com/wp-content/uploads/2014/05/flex-grow.svg)

``` css
.item {
  flex-grow: <number>; /* default 0 */
}
```

> 주의: 음수는 적용되지 않는다.


#### flex-shrink

`flex-shrink`는 반대로 줄어는다.

``` css
.item {
  flex-shrink: <number>; /* default 1 */
}
```

> 주의: 음수는 적용되지 않는다.

#### flex-basis

`flex-basis`는 남은 공간을 분배하기 전에 기본 자식의 크기를 지정한다.
이 속성은 20% 라던가, 12px 같은 길이값으로 값을 지정할 수 있다.

`auto`키워드를 사용하면 width나 height 속성을 보고 지정된다.

`content`키워드를 사용하면 자식의 크기를 기반으로 해서 지정된다.
그치만 이 키워드는 아직 정상적으로 잘 지원하고 있는것 같지는 않다고 하네요.

``` css
.item {
  flex-basis: <length> | auto; /* default auto */
}
```

만약에 `0`으로 설정되어있으면, 남은 공간이 따로 설정되지 않는다.
만약에 `auto`로 설정되어 있으면, 남은 공간이 `flex-grow` 속성을 보고 분배될것이다. 

#### flex

`flex`속성은 `flex-grow`와 `flex-shrink`와 `flex-basis`를 한번에 쓴것이다.
`flex-shrink`와 `flex-basis`는 옵션으로 써도되고 안써도 된다.

``` css
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```

이 속성은 각각의 속성들은 좀더 짧게 쓰고싶을때 사용한다.
~~근데 진짜 필요한 속성인지는 잘 모르겠다... 오히려 이걸 모르면 하나하나 찾아봐야되는거기도 하고...? 음... 모르겠다ㅠ~~

#### align-self

`align-self`는 각각의 자식들 스스로가 정렬 규칙을 정할 수 있게 한다.

`align-items`은 부모 container에서 모든 자식에게 정한 규칙이라면
`align-self`는 자식 스스로가 자신의 규칙을 정하는것이다. 
따라서 `align-self`의 우선순위가 더 높다

![align self](//css-tricks.com/wp-content/uploads/2014/05/align-self.svg)

``` css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

> 이 속성은 `float`, `clear`, `vertical-align`에 영향을 주지 않는다.


## 참고

> [https://css-tricks.com/snippets/css/a-guide-to-flexbox/](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)