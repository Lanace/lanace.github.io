# lanace.github.io

> 개인용 블로그 구축을 위한 프로젝트 입니다.
> 내가 하고싶은대로... 이것저것 가져다 써보기도 하고 그러는 실험실 같은 프로젝트
> ~~좀 이쁘게 만들고싶다ㅠㅠㅠ~~

## 개요

Vue.js를 공부할 겸 개인 프로젝트로 블로그 개발을 위해 만들어진 프로젝트
학습용이기 때문에 실제로 릴리즈하여 서비스 하기엔 어려움이 있을 수 있음 (~~하지만 사용하겠다면 말리진 않을게요~~)
평소에 Markdown을 사용해서 글을 쓰다보니 Markdown 기반의 블로그를 만들게 됨
또한 지금까지 해왔던 프로젝트 정보, 이력서 등 경력관리할 수 있는 컨텐츠를 구현함
초기 구축은 반드시 필요한 내용으로 `홈`, `프로젝트`, `이력서`, `블로깅` 으로 구성하였음
추후에 관리자 페이지를 추가할 예정

## 요구사항

> ~~원래는 따로 요구하는 요구사항이 없었으나 개인적으로 지금까지 생각해왔었던 요구사항을 토대로 프로젝트를 진행하고자 함~~

1. 다양한 기술을 사용하여 기능을 구현하되, 반드시 필요한 부분에서만 사용할것.
1. Vue를 기반으로 한 생태계를 경험.
1. 개발자가 블로그를 관리하는데 편리한 기능을 추가할것.
1. 디자인은 크게 신경쓰지 않음. 하지만 주요 브라우저에서는 동작할것. (IE, Chrome, Edge, Safari)
1. MarkDown을 사용한 관리 및 운영
1. 지속 가능한 개발 형태

## install

## build

``` bash

bundle exec jekyll build

bundle exec jekyll serve

```

## usage

### 외부 플러그인 추가

1. gemfile에 의존성 추가
2. bundle install
3. bundle exec jekyll build


```
gem "jekyll-paginate"
bundle install
bundle exec jekyll build
```

## 사용 테마

[Skinny Bones Jekyll Starter] (http://mmistakes.github.io/skinny-bones-jekyll/)
