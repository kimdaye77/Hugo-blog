---
title: "[TS] Type script 공부 #1"
date: 2023-07-27T18:06:48+09:00
draft: false
slug: "type-script1"
tags: ["타입스크립트", "리액트", "노마드코더"]
---

## Type script 공부 #1

[노마드코더 강의 링크](https://nomadcoders.co/react-masterclass/lobby)
<br>
오늘은 어제 뉴스스탠드 기능 구현을 완료했으므로 예외 처리 간단한 거 3개만 해주고 8월에 있는 프로젝트를 위해 노마드코더의 타입스크립트 강의를 봤다.
<br><br>
강의를 보고 클론 코딩하면서 메모하고 싶은 개념들을 아래에 정리하겠다.

### string interpolation

하하 이거는 전에도 `interpolation`이 무슨 뜻일까? 궁금해서 검색해봤는데 정리를 안하니까 까먹은 개념이다. 정확히 말하면 알던 개념이지만 이것을 `string interpolation`이라고 하는지 몰랐다. 이제는 기억하기 위해 적어놓는다.

```js
# string interpolation
${변수명}
```

**문자열 인터폴레이션** 은 해당 값으로 대치된 결과를 생성하는 과정이다. 위의 코드에서는 해당 변수명의 값으로 대체된다.
(비슷하게 변수 인터폴레이션, 변수 대체, 변수 확장라고도 한다.)
js에서 굉장히 많이 쓴다..!

### 타입스크립트

대망의 타입스크립트..!
<br>말로만 듣고 처음 본다. 타입스크립트는 자바스크립트를 기반으로 한 프로그래밍 언어로 자바스크립트와는 다른 언어이다. 하지만 기반으로 했기 때문에 몇 가지 차이점을 제외하고는 비슷하다.
<br><br>타입스크립트의 두드러지는 특징으로는 다음과 같다.

#### strongly-typed 언어

이름에서부터 알다싶이 타입스크립트는 코드를 작성할 때 타입을 명시해야한다. 그러면 타입스크립트는 프로그래밍 언어가 작동하기 전에 type을 확인한다.
**작동 원리**
브라우저는 js밖에 이해하지 못하므로 ts는 컴파일하면 js로 바뀐다. 따라서 `protection`은 코드 작동 전 발생한다. 코드 작성 전에 타입이 맞는지 보고 문제가 없으면 js 코드를 리턴한 것을 브라우저가 띄어준다.

#### 번외

js에서 쓰는 라이브러리들은 타입들을 ts가 알 수 있게 해줘야한다. 이때,
`@types`를 사용해서 ts에게 타입을 알려줄 수 있다. 거대한 github repository 유명한 npm 라이브러리를 가지고 있는 저장소로 여기서 라이브러리나 패키지의 type definition 을 알려준다.

#### 타입스크립트 장점

1. props 오타 등의 실수를 알 수 있다.
2. 타입 틀렸을 때 컴파일 전에 알 수 있다.

#### ?? Null 병합 연산자 (Nullish coalescing operator)

`??` 앞에 값이 null이거나 undefined이면 오른쪽 값을, 그렇지 않으면 왼쪽 값을 반환하는 논리연산자

```ts
# 예시
null ?? "hello" // "hello"
undefined ?? "hello" // "hello"
"hi" ?? "hello" // “hi"
```

#### currentTarget과 target 차이

https://choonse.com/2022/01/14/605/
<br>

- typescript에서 reactjs는 `currentTarget` 사용을 택했다.

#### ES6 문법

https://nomadcoders.co/react-masterclass/lectures/3336 -몰랐던 신기한 es6 문법

```js
const {
  currentTarget: { value },
} = event;
```

event안 curentTarget안에 value의 값을 기존 이름 그대로 value 라는 변수를 만든다.
`const value = event.currentTarget.value` 랑 같은 의미이다.
<br>왜 사용할까?
<br>currentTarget안에서 value, tagName, width, id 등 **여러 개** 를 가져오고 싶을 때 장점이 있다.

```js
# 기존 문법
const value = event.currentTarget.value;
const tagName = event.currentTarget.tagName;
const width = event.currentTarget.width;
const id = event.currentTarget.id;
```

이거를 이렇게 바꿔 쓸 수 있다.

```js
# ES6 문법
const {
currentTarget: {value, tagName, width, id}
} = event;
```
