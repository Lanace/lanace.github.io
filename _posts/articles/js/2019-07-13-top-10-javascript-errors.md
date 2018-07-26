---
layout: article
title: "Top-10-javascript-errors"
categories: articles
modified: 2016-07-13
tags: [js]
comments: true
ads: true
image:
  feature: crossbrowsing.png
  teaser: crossbrowsing.png
  thumb: crossbrowsing.png
---

## document.getElementByName 

[document.getElementByName](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByName) 는 Dom에서 name속성으로 원하는 element를 가져올 수 있다.
스팩상 `__proto__`가 `NodeList`인 형태로 값을 가져오는데 `IE`와 `Edge`에서는 Object로 가져오는것을 확인할 수 있다.

![IE에서의 document.getElementByName __proto__ 이미지](/images/getElementByName_proto_ie.png)

- IE에서의 document.getElementByName __proto__ 이미지

- Edge에서의 document.getElementByName __proto__ 이미지

![IE에서의 document.getElementByName __proto__ 이미지](/images/getElementByName_proto_chrom.png)

- Chrome에서의 document.getElementByName __proto__ 이미지

따라서 이렇게 `getElementByName`으로 가져온 값을 forEach하려 할때 브라우져별로 동일하게 동작하지 않을 수 있다.

가능하면 기본적인 `for`문을 사용하거나, `foreach`나 `for in` 을 사용하려면 강제로 형변환이 필요하다.

하지만 하위 브라우저나 여러 다양한 브라우저를 지원하기 위해서 기본적인 for문을 사용하는것이 가장 좋아보인다.

## new URL()

[URL](https://developer.mozilla.org/en-US/docs/Web/API/URL#AutoCompatibilityTable) 은 보통 `URL`을 javascript에서 params를 얻어오기 위해서 사용하고 있었는데
아래와 같이 사용하고 있었다.

```javascript
// window.location.href = 'lanace.github.io?type=3';

var url = new URL(window.location.href);
url.searchParams.get('type');	// 3
```

그치만 IE에서는 `URL`을 지원하지 않을 뿐더러 문서에는 지원한다고 했던 Edge 에서도 정상적으로 동작하지 않았다.
그래서 [stackOverFlow](https://stackoverflow.com/a/48447730/5949460)의 답변을 참고하여 수정하였다.

```javascript
function decodeUriComponentWithSpace (component) {
    return decodeURIComponent(component.replace(/\+/g, '%20'))
  }

  // type : 'hash', 'search' or 'both'
  function getLocationParameters (location, type) {
    if (type !== 'hash' && type !== 'search' && type !== 'both') {
      throw 'getLocationParameters expect argument 2 "type" to be "hash", "search" or "both"'
    }

    let searchString = typeof location.search === 'undefined' ? '' : location.search.substr(1)
    let hashString = typeof location.hash === 'undefined' ? '' : location.hash.substr(1)
    let queries = []
    if (type === 'search' || type === 'both') {
      queries = queries.concat(searchString.split('&'))
    }
    if (type === 'hash' || type === 'both') {
      queries = queries.concat(hashString.split('&'))
    }

    let params = {}
    let pair

    for (let i = 0; i < queries.length; i++) {
      if (queries[i] !== '') {
        pair = queries[i].split('=')
        params[this.decodeUriComponentWithSpace(pair[0])] = this.decodeUriComponentWithSpace(pair[1])
      }
    }
    return params
}

   // TEST: 
window.location.hash = 'test=a&test2=b'
console.log(getLocationParameters(window.location, 'both'))
```

위 코드는 stackoverflow에서 가져온 코드이다. 당연한거지만 코드를 가져오더라도 어떻게 동작하는지, 내부적으로는 어떻게 돌아가는지 정도는 확인해보고 사용해야한다. 

위 코드 에서 조금 이상해보이는 부분이 있는데 바로 `decodeUriComponentWithSpace` 함수이다. 이 함수는 `component.replace(/\+/g, '%20')` 하고 있는것을 볼 수 있는데 문자열에서 `+` 기호를 모두 %20으로 바꾼다는것이다. 
이것이 의미하는게 무엇일까.


## URL.search

[new URL()](#new URL()) 문제를 해결하고나니 연관된 이슈로 search params를 일부 수정해야되는 경우가 생겼다.
기존에 URL을 사용해서 `url.search` 를 하면 자동으로 search params가 나왔는데 이를 사용할 수 없게된 것이다.

즉 아래와 같이 사용할 수 없었다.

```javascript
var url = new URL(window.location.href);
url.searchParams.set('type', 1);
url.searchParams.set('page', 1);

return url.pathname + url.search;
```

따라서 직접 구현해서 사용하기로 하였다.
우선 `new URL()` 문제는 위에서 `getLocationParameters()` 함수를 사용해 params object를 가져오고,
가져온 object에서 `type`과 `page`를 수정하였다.

그리고 object를 search params로 만드는 작업은 아래의 코드를 이용하였다.

```javascript
var objectToSearchParams = function(obj, prefix) {
  var str = [],
    p;
  for (p in obj) {
    if (obj.hasOwnProperty(p)) {
      var k = prefix ? prefix + "[" + p + "]" : p,
        v = obj[p];
      str.push((v !== null && typeof v === "object") ?
        serialize(v, k) :
        encodeURIComponent(k) + "=" + encodeURIComponent(v));
    }
  }
  return str.join("&");
}
```

이후 return 부분에서 `window.location.pathname` + `objectToSearchParams(url)`; 하여 해결하였다. 

## Array.prototype.includes

[Array.prototype.includes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes) 에서 볼 수 있드시 IE에서는 지원하지 않는다.

문서에서 `polyfill`을 설명한대로 적용하여 해결하였다.

```javascript
// https://tc39.github.io/ecma262/#sec-array.prototype.includes
if (!Array.prototype.includes) {
  Object.defineProperty(Array.prototype, 'includes', {
    value: function(searchElement, fromIndex) {

      if (this == null) {
        throw new TypeError('"this" is null or not defined');
      }

      // 1. Let O be ? ToObject(this value).
      var o = Object(this);

      // 2. Let len be ? ToLength(? Get(O, "length")).
      var len = o.length >>> 0;

      // 3. If len is 0, return false.
      if (len === 0) {
        return false;
      }

      // 4. Let n be ? ToInteger(fromIndex).
      //    (If fromIndex is undefined, this step produces the value 0.)
      var n = fromIndex | 0;

      // 5. If n ≥ 0, then
      //  a. Let k be n.
      // 6. Else n < 0,
      //  a. Let k be len + n.
      //  b. If k < 0, let k be 0.
      var k = Math.max(n >= 0 ? n : len - Math.abs(n), 0);

      function sameValueZero(x, y) {
        return x === y || (typeof x === 'number' && typeof y === 'number' && isNaN(x) && isNaN(y));
      }

      // 7. Repeat, while k < len
      while (k < len) {
        // a. Let elementK be the result of ? Get(O, ! ToString(k)).
        // b. If SameValueZero(searchElement, elementK) is true, return true.
        if (sameValueZero(o[k], searchElement)) {
          return true;
        }
        // c. Increase k by 1. 
        k++;
      }

      // 8. Return false
      return false;
    }
  });
}
```

## 정리

아직 완벽하게 `ie`와 `chrome`을 지원하는건 아니지만 최소한 위에서 찾았던 문제점들에 대해서는 작업할때 참고해서 작업해야겠다.
`chrome` 점유율이 점점 더 높아지고 있긴 한데, 아직 `edge`나 `ie`를 사용하는 사용자도 많다ㅠ ~언제쯤 갈아타실꺼죠!~ 
여튼 신기술을 좋아하긴 하지만 이런 부분에서는 약간 단점도 있는것같다.

적절하게 사용하는게 중요한듯...

계속해서 버그 잡으면서 업데이트 해야겠다.
