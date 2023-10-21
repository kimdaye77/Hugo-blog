---
title: "[FE] 라우팅"
date: 2023-08-08T21:28:59+09:00
draft: false
categories: "프론트엔드"
slug: "routing"
tags: ["프론트엔드"]
---

## Routing

오늘은 라우팅에 관한 수업을 들었다. 라우팅과 관련해서 이야기하면서 SPA(Single Page Applications)와 history api에 대해서도 공부하였다.
현재 지금 개발중인 프로젝트(Cartag)이 SPA로 구현하고 있기 때문에 라우팅은 굉장히 중요한 개념이다. 오늘 배운 내용을 정리하겠다.

### SPAs?

![SPA vs MPA](https://velog.velcdn.com/images/gwanuuoo/post/b73c016b-81d6-46f4-a7a7-adc7e5fd46ff/SPA-MPA.png)
먼저 SPA란 Single Page Application의 줄임말이다. 대부분의 웹사이트는 MPA(Multiple Page Application)으로 되어있다.
하지만 최근들어 SPA로 개발하는 것이 트렌드인 듯 싶다. 이렇듯 SPA로 개발하면 어떤 특징이 있는지 알아보자.

#### SPA 특징

우선, SPA는 모든 정적 리소스를 초기에 로딩한다. 따라서 페이지 간 이동 시, 페이지 갱신에 필요한 데이터만을 JSON으로 전달받는다는 것이 가장 큰 특징이다.<br>
SPA는 React에서 Router를 통해 구현할 수 있다.

### Router

SPA에서 routing은 사용자가 쉽게 navigate할 수 있게끔 도와준다.
왜냐하면 SPA는 하나의 페이지를 보여주지만 주소에 따라 각기 다른 페이지를 보여주기 때문이다.
주소에 따라서 보여지는 화면이 달라지게 해주는 기술을 리액트에서 `Routing`을 통해 지원해준다.

### history API

history API는 브라우저의 세션 history를 관리할 수 있는 메소드를 담고 있는 객체이다. 이를 이용해서 뒤로 가기 및 페이지 이동을 할 수 있다.

```js
# history API 메소드

history.back(); // 뒤로
history.forward(); // 앞으로
history.go(); // 새로고침
history.go(-1); // 뒤로
history.go(1) // 앞으로

# html5부터 추가된 메소드
history.pushState({데이터 객체}, title, page)
history.replaceState({데이터 객체}, title, page)
```

#### history 비교

- 카카오 맵: 히스토리는 쌓이는데 상태 관리가 안 된다.
- 구글 맵: 히스토리도 쌓이고 상태 관리 된다.

=> 쿠팡 사이트 때부터 느낀건데 대기업이 개발한 페이지여도 ux가 별로인 것이 상당히 많았다.
