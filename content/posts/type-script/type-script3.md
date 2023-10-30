---
title: "[TS] Type script 공부 #3"
date: 2023-07-29T22:00:45+09:00
draft: false
categories: "타입스크립트"
slug: "type-script3"
tags: ["타입스크립트", "리액트", "노마드코더"]
---

## Type script 공부 #3

[노마드코더 강의 링크](https://nomadcoders.co/react-masterclass/lobby)
<br>
오늘도 이어서 타입스크립트 공부를 시작했다. 어제까지 타입스크립트 개념 강의는 모두 끝내고 오늘부터는 3가지 클론 코딩 프로젝트 중 **암호화폐 시세 트래커**를 시작한다.

👇🏻 아래는 클론코딩용 깃허브 레포지토리 링크이다. 👇🏻<br>
[깃허브 링크](https://github.com/kimdaye77/crypto-tracker)

### 전역 style

#### reset.css

브라우저의 기본 스타일들을 초기화해주는 css 파일로 기본값을 제거해주는 역할을 한다. 보통 **1. 인터넷에 검색하면 나온 것을 복사+붙여넣기 해서 `reset.css` 파일**을 만들거나
**2. `npm i styled-reset` 명령어로 설치**할 수 있다.

#### createGlobalStyle

전역 스타일을 처리하는 특수 Styled Component를 생성하는 helper 함수이다.
한 컴포넌트를 만들 수 있게 해주는데 렌더링될 때 이 컴포넌트는 전역 스코프에 스타일들을 알려준다.

이 때 우리는 두 개의 `element`를 써야하는데
`<GlobalStyle/><Router/>` 리액트는 하나만 리턴해야 한다.
if, `<div>`로 감싸면 어플리케이션은 쓸모없는 div가 많아지므로 **bad practice**이다.

#### fragment

일종의 유령 컴포넌트이다. 부모없이 서로 붙어있는 많은 엘리먼트들을 리턴할 수 있게 해준다.

### API의 인터페이스

타입스크립트에서는 API도 인터페이스를 설정해줘야 한다. ㅎㅎ
물론, API의 type에 대한 정보를 자동생성하는 좋은 사이트도 존재하지만(이건 후술하겠다.)이번 강의에서는 직접 만들기 때문에 해보겠다.

#### type 정보 생성

![첫 번째](img/typescript3-api1.png)

1. 우선 `console.log`로 얻은 데이터를 콘솔에 출력한다. 그리고 콘솔창에서 우클릭을 하면 전역 변수로 Object 저장을 눌러준다.

![두 번째](img/typescript3-api2.png)
![세 번째](img/typescript3-api3.png)

2. 1번 과정을 끝내면 위와 같이 temp로 저장된다. 저장한 객체를 `keys.join()`하면 string으로 나온다.

![네 번째](img/typescript3-api4.png)

3. 위의 문자열을 복사해서 붙여놓고 `Object.values(temp1).map(v => typeof v).join()`로 타입 또한 결과를 얻어서 붙여넣으면 완성!

#### issue

react-router-dom v5 버전 사용 시 URL 은 변하는데 렌더링이 안되는 이슈가 있다. 나 또한 강의를 따라하느라 같은 이슈가 발생했다. (강의에서는 버전 5 사용, 최신은 버전 6이기 때문에 차이가 있다.)

<mark>**1. index..tsx 에서 렌더 내부의 React.StrictMode 를 div 로 변경**</mark> <br>2. react-router-dom v6을 사용

나는 1을 사용해서 해결했다.

### 각종 소소한 정보 & Tips

#### 정보

1. coininterface[]는 CoinInterfaces의 배열을 의미한다.
   tip
2. (함수)(); 이렇게 작성하면 바로 실행된다. -> **즉시 실행 함수** 참고

3. 비하인드 더 씬

`Link`를 통해 `state`를 보낼 수 있다. `to`에 경로만 적지 말고 {} 객체로 감싸서 보내면 데이터까지 전송할 수 있다.

```ts
<Link
    to={{
    pathname: `/${coin.id}`,
    state: { name: coin.name },
}}
```

데이터를 받는 컴포넌트에서는 아래와 같이 `useLocation`을 사용해서 사용하면 된다.

```ts
interface RouteState {
name: string;
}

const {
state: { name },
} = useLocation<RouteState>(); >
```

하지만, 아래의 상황에서 문제가 발생할 가능성이 있다.

&rarr; 시크릿창일 때 링크의 데이터를 못 받는다.
<br>
&rarr; 링크로 다이렉트로 검색할 때
<br>
<br>
위의 예외 처리를 해주어야 한다.

4. 캡슐화

```ts
const response = await(
  await fetch(`https://api.coinpaprika.com/v1/coins/${coinId}`)
).json();
```

5. 타스 코드베이스를 보면 가끔 interface를 적을 때 I를 맨앞에붙이고 하는 경우 왕왕 있다.

6. 특수 문자 코드표

`&rarr;` 이렇게 작성하면 오른쪽 화살표를 의미한다. [참고](https://www.w3.org/TR/WD-math-970515/table06.html)

#### Tip 🍯

```
# VSCode 단축키

Ctrl(Command)+D: 같은 문자열 선택
Shift+Alt(Option)+i: 선택한 모든 문자열에 가장 우측 끝으로 포커싱
Ctrl(Command)+Shift+오른쪽 화살표: 현재 선택한 문자열을 기준으로 우측 끝까지 문자열 선택
```

[노마드코더 코딩 인생 꿀템 VSC 단축키 5분 정리](https://www.youtube.com/watch?v=Wn7j5dfbJF4)
<br>
[JSON데이터를 타입스크립트 타입으로 빠르게 변환시켜주는 사이트
](https://app.quicktype.io/?l=ts)
