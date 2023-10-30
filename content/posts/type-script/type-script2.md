---
title: "[TS] Type script 공부 #2"
date: 2023-07-28T17:55:14+09:00
draft: false
categories: "타입스크립트"
slug: "type-script2"
tags: ["타입스크립트", "리액트", "노마드코더"]
---

## Type script 공부 #2

[노마드코더 강의 링크](https://nomadcoders.co/react-masterclass/lobby)
<br>
사실 오늘은 개발 시간이 없고 기획+디자인 리뷰와 데모 영상 시연으로 공부를 거의 하지 못했다. 따라서 어제 정리한 내용이지만 분량 상 적지 않았던 것을 적어보고자 한다.

### interface

interface는 object 설명해주는 것이라고 보면 된다. 인터페이스는 변수의 타입으로 사용할 수 있다. 이때 인터페이스를 타입으로 선언한 변수는 해당 인터페이스를 준수하여야 한다. 만약 `required`가 아닌 변수는 다음과 같이 `?`를 뒤에 붙여 `optional props`로 선언해주면 된다.

```js
interface Exxample {
  required: string;
  optional?: string; // props가 선택적으로 입력되는 경우
}
```

### 인터페이스 관련 좋은 예시

`PropTypes`(리액트 라이브러리)와 비슷하지만 `interface`는 <mark>코드가 실행되기 전</mark> 확인해준다.

```js
# Player.js
interface PlayerShape {
    name: string;
    age: string;
}

const sayHello = (playerObj: PlayerShape) => `Hello ${playerObj.name}. You are ${playerObj.age} years old.`;

sayHello({name:"nico", age:12})
```

위에서 이전에 정리한 화살표 함수 표현식이 사용되었다..!
<br>
[참고:[현대 소프티어] FE 뉴스스탠드 #2](https://kimdaye77.github.io/posts/fe-newsstand/fe-newsstand2/)
<br>여기서 **[번외] 화살표 함수**로 이동해서 보면 된다.

### interface를 통한 다크모드 구현

```ts
# styled.d.ts

import "styled-components";

declare module "styled-components" {
  export interface DefaultTheme {
    textColor: string;
    bgColor: string;
    btnColor: string;
  }
}
```

`d.ts`는 declaration file 이라는 뜻이다. `d.ts`로 된 확장자를 통해 `styled-components`에 `DefaultTheme` 인터페이스를 확장해준다. 이제 애플리케이션에서 사용하는 테마의 형태를 정의할 수 있다. 예를 들어, 이 코드에서는 DefaultTheme에 textColor, bgColor, 그리고 btnColor라는 세 가지 속성이 추가되어 테마의 색상을 관리할 수 있도록 한다.

```ts
# theme.ts

import { DefaultTheme } from "styled-components";

export const theme: DefaultTheme = {
  bgColor: "white",
  textColor: "black",
  btnColor: "tomato",
};
```

`theme.ts`에서 테마를 해당 인터페이스 props에 대응하는 테마를 만들고 `index.tsx`에서
다음과 같이 작성해주면 된다.

```tsx
<ThemeProvider theme={theme}>
  <App />
</ThemeProvider>
```

이렇게 `App`에게 전해주고 `App`에서 해당 테마를 사용하면 완료!
매우 간편하다. 이제 타입스크립트 개념 강의는 끝나서 실제 프로젝트를 클론 코딩한다. 두근두근
