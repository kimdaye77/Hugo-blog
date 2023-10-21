---
title: "[현대 소프티어] FE 뉴스스탠드 #2"
date: 2023-07-18T14:57:48+09:00
draft: false
categories: "현대 소프티어"
slug: "fe-newsstand2"
tags: ["프론트엔드", "newsstand", "현대소프티어"]
---

## FE 뉴스스탠드 3주차

참고 {{< icon "github" >}}
<br>
[fe-newsstand #3](https://github.com/softeerbootcamp-2nd/fe-newsstand/pull/80)
<br>
[fe-newsstand 7.18](https://github.com/kimdaye77/fe-newsstand/pull/5)
<br>

### Feature List

#### 남은 기능

(오늘 구현한 기능은 형광펜 표시)

- [x] 구독하기/해지하기 버튼
- [ ] 리스트 뷰 메인 로고 썸네일에 마우스 호버 시 5% 확대
- [x] 구독한 언론사 탭 버튼 이벤트
- [x] <mark>구독한 언론사 페이지 구현</mark> {{< icon "check" >}} + 구독/해지 이벤트

- [ ] 그리드 뷰 마지막 페이지 언론사 로고 갯수 96개 아닐 때 그리드 표시
- [ ] 다크모드 (추가 미션)

#### 개발한 기능 시연

<video controls>
  <source src="
https://github.com/kimdaye77/Hugo-blog/assets/63107805/fc3e84e4-f891-4f79-983d-2c53bfbf7ae3" type="video/mp4" />
</video>

구독/해지 이벤트를 구현하는 것까지 완료하였으나 지금 코드가 스파게티 코드 상태라 리팩토링을 빡세게 진행해야 한다.

## 학습한 내용

주차 중 화요일, 수요일은 자바스크립트 관련 수업을 듣는다. 이번 수업에서는 scope, closure, 비동기, promise, json 객체 등 얼핏 알고 있었지만 자세히는 몰랐던 개념에 대해 알았다. 수업시간에 코드 예시를 실행하면서 원리를 알았지만 아직 내 지식으로 습득은 안 된 것 같아서 집에 돌아가면 다시 천천히 봐야겠다.

### Scope

#### Scope chaining

![Scope-chaining](https://cdn.hashnode.com/res/hashnode/image/upload/v1610133490141/DgG9zwudY.png?auto=compress,format&format=webp)

함수가 해당하는 값을 `block -> closure` 이런 식으로 올라가면서 scope를 올라가면서 찾는다.<br>
`var` 함수 레벨 scope<br>
`let` block 레벨 scope

### 비동기와 동기

```js
const baseData = [1, 2, 3, 4, 5, 6, 100];

const asyncRun = (arr, fn) => {
  for (var i = 0; i < arr.length; i++) {
    setTimeout(() => fn(i), 1000);
  }
};

asyncRun(baseData, (idx) => console.log(idx));
```

수업시간에 다룬 예제 코드 중 하나이다. 결과는 무엇일까?

<details> <summary>정답</summary>7이 7번 출력된다.</details>

**여기서 `var`를 `let`으로 변경하면?**
<br>
<br>`var`는 **함수 level scope**, `let`은 **block level scope**이다.
<br>따라서 `var`일 경우 참조값이 closure에 보관되어 있고 이 값은 for문을 7번 다 돈 후의 i이다. 즉 `asyncRun`의 지역변수 i (값은 7)를 참조한다.
`let`의 경우에는 block level이기 때문에 각 for문 하나가 call stack에 쌓인다.
<br>
![사진 참고](https://slack-imgs.com/?c=1&o1=ro&url=https%3A%2F%2Fres.cloudinary.com%2Fpracticaldev%2Fimage%2Ffetch%2Fs--kRxN-sBc--%2Fc_imagga_scale%2Cf_auto%2Cfl_progressive%2Ch_500%2Cq_auto%2Cw_1000%2Fhttps%3A%2F%2Fthepracticaldev.s3.amazonaws.com%2Fi%2Fek7ji4zrimozpp2yzk0a.png)

### Promise

```js
let myFirstPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Success!");
  }, 1000);
});
myFirstPromise.then((successMessage) => {
  console.log("Yay! " + successMessage);
});
```

```
1. Promise객체
2. setTimeout 함수
3. setTimeout에 전달되는 콜백
4. then 함수
5. then에 전달되는 콜백
6. resolve 함수
7. Promise 함수에 전달되는 콜백
```

중에 실행 순서는 어떻게 될까?

<details> <summary>정답</summary>1 7 2 4 3 6 5</details>

**실행순서**  
`promsie 실행 > then 실행(콜백함수 promise에 등록) > 비동기 완료시 resolve 메서드 실행 > 콜백함수 실행`

then은 동기함수이다. resolve의 신호가 있어야 콜백을 실행한다.
<br><mark>**fulfilled** = 비동기 처리 끝났으니 실행하면 된다는 뜻!</mark>  
promise.then은 한 세트로 큐에 같이 들어간다. 또한 다른 큐에 쌓인 함수보다 우선순위가 높다.

### [번외] 화살표 함수

이번 수업시간은 아니고 크롱님(수업 담당하시는 분의 닉네임)께서 저번 주차에 숙제로 알아오라고 하셨다. ~~(2번이나...!)~~

```js
// 예시 함수
const exampleFunction = (parameter) => {
  // 실행할 내용
};
// 이벤트 리스너
document.addEventListener("click", exampleFunction(parameter));
```

위의 경우, 가독성 좋게 아래와 같이 변경할 수 있다.

```js
// 예시 함수
const exampleFunction = (parameter) => () => {
  // 실행할 내용
};
// 이벤트 리스너
document.addEventListener("click", exampleFunction(parameter));
```

## 향후 계획

유용한 사이트를 많이 알았다. 오늘 학습한 call stack, event loop, queue와 앞으로 적용할 디자인 패턴 같은 내용이 사이트에 포함되어 있다.

### 유용한 사이트

1. [JavaScript 개념 사이트](https://dev.to/lydiahallie/javascript-visualized-event-loop-3dif)
2. [디자인 패턴 모음](https://patterns-dev-kr.github.io/)

### 추후 할 것

- 구독한 언론사 탭에서 해지하면 바로 없어지도록 구현하기
- 리팩토링  
  -- store.js, 옵저버 패턴  
  -- promise 객체 활용
