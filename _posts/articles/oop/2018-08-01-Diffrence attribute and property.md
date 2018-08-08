---
layout: article
title: "Attribute와 Property의 차이"
categories: articles
modified: 2016-07-11
tags: [develop, oop, attribute, property]
comments: true
ads: true
image:
  feature: 
  teaser: 
  thumb: 
---

> 평소에 자주 쓰는 용어들중 서로 비슷한 의미로 사용하는 경우가 많이 있다.  
> 그중에 속성이라는 말을 평소제 자주 쓰는데 영어로 보통 `Attribute`나 `Property`라고 한다.  (~~가끔 `field` 라는 용어도 쓰긴 한다~~)  
> 그렇다면 이 두 단어는 각각 어떤 의미가 있고, 차이가 있는지 궁금해서 알아보게 되었다.  

## Attribute

`Attributes`는 element가 가지고 있는것을 의미한다.

`Attributes`는 HTML 요소의 추가적인 정보를 전달하고 이름=“값” 이렇게 쌍으로 온다. 예를 들어 <div class=“my-class”></div> 를 보면 div 태그가 class 라는 값이 ‘my-class’인 attribute를 가지고 있다.

예를들어, HTML에서의 `Attributes`는 element가 가지고 있는것을 의미한다.
`이름=“값”` 이렇게 Key-value로 되어있는것을 의미한다.

```html
<div class=“my-class”></div>
```

## Property

`Property`는 `attribute`에 대한 HTML DOM 트리안에서의 표현이다. 그래서 위 예시에서 attribute는 값이 ‘my-class’이며 이름이 ‘className’인 property를 가진다.

## 그 외의 이야기

### jQuery 1.6 / 1.6.1

jQuery에는 `.attr()`과 `.prop()` 가 존재하는데, 각각 attribute와 property에 접근할 때 사용한다. 
아래에는 jQuery에서 명시한 둘의 차이이다. 

> The difference between attributes and properties can be important in specific situations. 
> Before jQuery 1.6, the .attr() method sometimes took property values into account 
> when retrieving some attributes, which could cause inconsistent behavior. 
> As of jQuery 1.6, the .prop() method provides a way to explicitly retrieve property values, while .attr() retrieves attributes.
> 
> For example, `selectedIndex`, `tagName`, `nodeName`, `nodeType`, `ownerDocument`, `defaultChecked`, and `defaultSelected` should be retrieved and set with the `.prop()` method. 
> Prior to jQuery 1.6, these properties were retrievable with the `.attr()` method, but this was not within the scope of attr. 
> These do not have corresponding attributes and are only properties.

요약하자면 1.6버전 이전의 jQuery에서는 
`Attributes`는 속성 자체를 의미하고, `Property`는 속성의 값을 의미한다.

예를들어 설명하자면 아래 코드를 보자.

```html
<checkbox id="my-checkbox" type="checkbox" checked />
```

```javascript
var $checkbox = $('#my-checkbox');

alert($checkbox.attr('checked'));  // checked 속성의 값을 표시

alert($checkbox.prop('checked'));  // checked 프로파티값을 표시
```

결과는 각각 `.attr()` → `"checked"`와 `.prop()` → `true` 이 된다.
즉, `.attr()`는 속성 자체를 의미하고, `.prop()`는 해당 속성의 값이 된다.


개인적으론 `.prop()`로 작성하는게 더 좋아보인다. 
