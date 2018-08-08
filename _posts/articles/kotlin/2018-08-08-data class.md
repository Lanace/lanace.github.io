---
layout: article
title: "Kotlin Data class"
categories: articles
modified: 2016-08-08
tags: [kotlin, data class]
comments: true
ads: true
image:
  feature: 
  teaser: 
  thumb: 
---

## Data Classes

> Java에서 하나의 모델을 위해 여러가지 자료형을 하나로 뭉치기 위해 Class를 생성하는데, 이런 Class에는 기능적 또는 유틸적 함수 (메소드)를 포함한다. 
> Kotlin에서는 이러한 Class를 Data Class라고 한다.

```kotlin
data class User(val id: Int, val name: String)
```

컴파일러는 자동적으로 뒷따라오는 멤버들을 보고 다음 메소드들을 생성한다.

- equals() / hashCode()
- toString()
- componentN() functions
- copy()

> `primary constructor`는 class의 왼쪽 괄호에 들어가는 인자로 생성되는 생성자를 의미한다.
> 예를들어 위의 User data class의 경우 id와 name을 생성자로 하는것을 `primary constructor`라 한다.
> body에 있는 property는 `primary constructor`에 포함되지 않는다.

일관성과 관리성을 보장하기위해 생성된 data class의 코드들은 다음 요구사항을 만족시킨다.

- 초기 생성자는 최소한 1개 이상의 인자를 갖는다
- 모든 초기 생성자는 `val` 또는 `var`로 선언되어야 한다.
- Data class는 `abstract`, `open`, `sealed` 또는 `inner`일 수 없다
  
추가적으로, 멤버 생성은 아래 규칙을 따른다.

- 만약 `equals()`, `hashCode()`나 `toString()`같은 요소들이 data class의 body나 superClass의 final 요소로 있는경우, 그런 함수들은 생성되지 않고, 기존 구현된 함수로 사용된다.
- 만약 supertype이 open되어 있거나, 호환 가능한 타입으로 반환되는 `componentN()` 함수를 갖고있다면, 일치하는 함수가 생성되어진다. 만약 supertype의 함수가 호환이 불가능하거나, final이라 override할 수 없는경우, 에러가 발생한다.
- 이미 `copy()`를 갖고있는 타입으로 부터 나오는 data class는 Kotlin 1.2에서 deprecated 되었고, Kotlin 1.3에서 금지될것이다.
- 명확하게 제공되는 요소의 `componentN()`와 `copy()`는 허용되지 않는다.

### data class의 Body에 Property 정의

컴파일러는 `primary constructor` 내부에 정의된 property만 사용하여 자동으로 함수들을 생성한다.
자동생성되는 함수들에서 제외시키려면 body에다가 정의해야한다. 

```kotlin
data class User(val name: String) {
  var age: Int = 0
}
```

위의 data class에서 `name`만 `toString()`이나 `equals()`, `hashCode()`같은 자동으로 생성된 메소드에서 인자로 사용된다.
그리고 `component1()`가 있을것이다. (???) 

아래 예제를 보자.

```kotlin
data class User(val name: String) {
  var age: Int = 0
}

fun main(args: Array<String>) {
  val user1 = User("Jeon")
  val user2 = User("Jeon")

  user1.age = 20
  user2.age = 30

  println("user1 == user2: ${user1 == user2}")
  println("user1 with age ${user1.age}: ${user1}")
  println("user2 with age ${user2.age}: ${user2}")
}

```

이 코드를 실행시키면 놀랍게도 `user1`과 `user2`가 같다고 나온다.
이는 `equal()`을 호출할 때 `name`만 참여하기 때문에 같다고 나온다.

### Copying



### Destructuring

반-구조화하여 초기화 할 수 있다.

```kotlin
val user = User("Jeon", 26)
val (name, age) = user

// name = "Jeon"
// age = 26
```

## 일반적인 Data class

일반적인 라이브러리들은 `Pair`와 `Triple`을 제공한다. 대부분의경우, Data class는 코드를 좀더 읽기좋게 해주기 때문에 설계함에 있어서 좋은 선택이 될것이다.


## Pacelable 

아마 Android 하면서 데이터를 넘기기 위해 Model로 사용하고 있는 class에 `Parcelable`을 impliment해서 사용해왔을것이다.

kotlin에서는 좀더 쉽게 parcelable로 만들 수 있는데, 
