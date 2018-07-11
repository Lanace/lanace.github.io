---
layout: article
title: "OpenGraph를 통한 썸네일 지정"
categories: articles
modified: 2016-07-11
tags: [web, opengraph, seo]
comments: true
ads: true
image:
  feature: facebook-open-graph-overview.jpg
  teaser: facebook-open-graph-overview.jpg
  thumb: facebook-open-graph-overview.jpg
---

## 개요

내가 보고 있는 페이지를 공유하는 일이 참 많아졌다.
공유를 하다보면 나는 분명 링크만 보냈을 뿐인데 링크에대한 썸네일이나 제목, 설명글등 해당 페이지에 들어가지 않아도 간단한 정보들을 볼 수 있다. 

![카카오톡 썸네일](/images/thumb_kakao.png)

이러한것을 가능하게 하기 위해서 사용하는것이 `opengraph`이다. 
(~~사실 뭐 다른 방법들이 있을꺼라 생각하지만 가장 많이 사용되고있는 방법이다.~~)

해당 링크를 들어가서 어떤 정보들이 `title`인지, `description`인지, `image`인지 등을 가져오기엔 어떤 정보가 정확한지 판단하기란 쉽지 않다. 
그래서 이러한 정보를 `<head>` tag 안에 `meta`로 넣어두는데 링크를 공유하면 이 header에 `meta`를 통해서 필요한 정보를 가져온다.

그럼 이제 어떤 정보를 설정할 수 있는지 확인해보자.

## Open Graph Protocol

자세한 내용은 [Open Graph](http://ogp.me/) 에서 확인할 수 있다.

### Tag	Description

- `og:url`	페이지의 표준 URL (데스크탑 URL)
- `og:title`	콘텐츠 제목
- `og:description`	콘텐츠 설명. 미리보기에서 제목 아래에 표시
- `og:site_name`	웹 사이트의 이름 (주소 아님)
- `og:image`	콘텐츠를 공유 시 표시되는 이미지의 URL
- `fb:app_id`	페이스북 인사이트를 사용하기 위한 앱 아이디
- `fb:admins`	웹 사이트용 도메인 인사이트를 사용하기 위한 아이디

여기서 내가 필요한건 링크를 보냈을 때 나오는 이미지와 Title, 그리고 설명글이다.
각각에 필요한 정보를 다음과 같이 header에 올려주면 적용된다.

간단하게 예시로... title을 적용하려면 `<meta property="og:title" content="Lanace의 블로그">` 라고 header에 추가해주면 된다.

이제 Jekyll에 적용해보자.

## Jekyll에 적용하기

자 그럼 이제 Jekyll에 적용해보자. 

Jekyll에서는 ~~아마~~ `_layouts`에 `default.html`페이지가 있을것이다. 이 페이지가 따로 설정하지 않았다면 모든 페이지에 들어가는 틀을 정의한 곳인데, 이곳에 header가 존재한다.

그럼 아래 코드와 같이 있을것이다.

<script src="https://gist.github.com/Lanace/ee6897c33b38de4b58662da08e1c6d0a.js"></script>

여기서 `include open-graph.html` 이부분을 보면 되는데 따로 나뉘어져 있다.
`includes`에 `open-graph.html`로 들어가면 아래와 같은 코드를 볼 수 있을것이다. 

<script src="https://gist.github.com/Lanace/e7a34a48aa82e62eb10ec11b1fb183c4.js"></script>

크게 두 부분으로 나눌 수 있는데, `Twitter Cards` 와 `Open Graph` 이다.
아래 [주의사항](#주의사항) 에서도 말하겠지만 모든 사이트에서 `Open Graph`를 지원하진 않는다. 따라서 트위터같은 사이트에선 지원하는것을 찾아서 적용해야한다. 그건 다음에 찾아보도록 하자.

우선 여기선 open graph를 보자.

내가 필요한건 링크를 보냈을 때 나오는 이미지와 Title, 그리고 설명글이다.
따라서 적용할 `meta`는 `og:title`, `og:description`, `og:image` 이다.

이를 위해 위에 코드처럼 `property`에 이 속성들을 넣고, `content` 에 해당하는 값을 정한다. 위에 코드는 `liquid`를 이용한 코드이므로 원하는 값을 넣어주면 된다.
(~~동적으로 바꾸고싶다면 liquid로 값을 넣어주시면 됩니다~~)

## 주의사항

* 특정 서비스에서 og:image 의 프로토콜이 https: 인경우 인식하지 못하니 되도록 http: 를 사용해야한다. (~~근데 난 잘 적용된것같다;; 뭐지?~~)
* 기존 페이지에 캐시가 남아있을 수 있으므로 적용하는데 시간이 필요하다. 
 - 단, 페이스북은 캐시를 날려주는 기능을 제공해서 [여기](http://developers.facebook.com/tools/lint/) 로 들어가서 갱신하면 금방 적용할 수 있다.
* 일부 서비스는 `opengraph`가 아닌 다른 방법으로 이를 확인해야 한다. 트위터 같이....
