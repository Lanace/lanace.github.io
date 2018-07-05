---
layout: article
title: "Kotlin 기본 문법 정리"
categories: articles
modified: 2016-07-05
tags: [kotlin]
comments: true
ads: true
---

## 기본 문법

### 변수 생성

var / val

var: mutable value 선언
val: immutable value 선언

### 자료형

* Double
* Float
* Long
* Int
* Short
* Byte
* String

#### Type casting

`to~~~` 메소드를 이용
```
ex) 
- toLong()
- toInt()
...
```

### Getter & Setter

C#에 있는 getter와 setter와 유사함

ex) 
```
class Sample<T> {
  var list: List<T> = mutableListOf()
    set(value) {
      if (value.isNotEmpty()) {
        field = value
      }
    }

    get() = field
    
  val isEmpty: Boolean
    get() = this.list.isEmpty()
}

```

### private 변수 

setter나 getter에 private 키워드를 삽입한다.

```
class Test {
  var name: String = "userName"
    private set()
}
```

### laze vs 

#### laze

- 늦은 초기화로, 호출 시점에 초기화됨
- val과 함께 사용함

```
val name: String by lazy {
  "lazy initialized"
}
```

#### lateinit

- var만 사용 가능
- null 이나 초기값이 필요 없음
- 늦은 초기화 이므로, 초기화 전에 사용하면 오류
- 변수에 대한 setter/getter 사용 불가

```
lateinit var name: String
name = "lateinit initialized"
```

### Tip

#### String Templates

문자열 중간에 변수를 넣기 편함
문자열 중간에 `"${value}"` 를 사용할 수 있음


## Function

### 함수 생성

여러가지 형태로 축약해서 사용 가능

```kotlin
fun getName(): String {
  return "name"
}

fun getName(): String = "name"

fun getName() = "name"
```

### Extension Function

기존 타입에 함수를 확장해서 사용 가능
- class에 있는 함수가 우선순위가 더 높음



## Class

### 기본 구조

```
class 클래스이름 constructor(변수) {
}
class 클래스이름(변수) {
}

// primary constructor - constructor 생략 가능
class Sample constructor(val name: String, val age: Int) {
  // secondary constructors - 생략 불가
  constructor(name: String) : this(name, 0)
}

class Sample(val name: String, val age: Int = 0) {
  init {
    println("name: $name, age: $age")
  }
}
```

### 상속

상속을 위해선 open class로 생성해야함


### Data Class

모델을 위한 클래스로 앞에 data 키워드를 붙이면 됨

### Inner Class

Class 내부에 새로운 Class를 생성
- 앞에 inner 키워드를 붙이면 됨 
- 