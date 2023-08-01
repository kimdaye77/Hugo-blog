---
title: "[Project] Cartag 프로젝트 #1"
date: 2023-08-01T21:47:31+09:00
draft: false
slug: "프로젝트"
tags: ["타입스크립트", "리액트", "프로젝트"]
---

## Cartag 프로젝트 #1

오늘부터 본격적으로 개발에 들어간다. 🐜 두둥! 탁<br>
개발에 들어가기 앞서 초기 개발 환경 세팅과 프로젝트 색상, 폰트들을 정의했다.<br>
~~빠른 시간 내에 완료하고 개발할 줄 알았으나...~~

### Vite

우선 크롱님께서 CRA(Create React App)보다는 Vite을 추천한다고 하셨다. 나는 이게 무엇인지 몰랐으나 팀원 분이 알아서 초기 개발 환경 세팅은 수월했다.
집에 와서 따로 공부하는 시간을 가지고 정리해본다.<br>
핵심만 말하자면 **로컬에서 개발할 때 번들링을 하지 않고 ESM 방식을 사용하기 때문에 로컬 서버 구동 속도가 매우 빠르다.** 는 것이다.
그렇다면 여기서 번들링과 ESM 방식이 무엇인가.

#### 번들링

자바스크립트에서는 `import`, `export`를 사용해서 모듈 방식으로 개발을 한다.
따라서 모듈화 문법을 위한 장치가 필요하다. 모듈화를 위한 장치로 유명한 것으로 웹팩이 있다.
모듈화 문법을 사용해 단위로 묶는 것을 번들링이라고 한다. 아래 그림을 보면 이해가 쉽다.
![번들링](https://images.velog.io/images/dkdlel102/post/4b282081-52d9-4a47-bd86-32fa49a7db4f/image.png)

#### ESM

자바스크립트 네이티브 모듈로 <mark>번들러 없이</mark> 브라우저 자체에서 실행가능한 모듈 방식이다.

#### 실행 시간 비교

모듈 번들러 중 웹팩을 예로 들어서 500개 정도의 모듈을 갖는 웹 서비스를 vite와 비교했을 때, 실행 시간은 vite가 20~30배 빠르다고 한다.<br>
이유는, 웹팩 데브 서버는 로컬 서버를 처음 시작할 때 관련 있는 모듈을 모두 번들링하여 메모리에 적재한다.<br>
그에 반해 비트는 시간을 소요하는 번들링 작업 없이 바로 서버를 실행하기 때문에 실행하기까지 많은 시간이 차이가 난다.

### Styled-component

#### Mixin css props

- css props는 자주 쓰는 css 속성을 담는 변수이다.
- 전역 변수이기 때문에 컴포넌트 어디든 글로벌에서 정의한 mixin 기능을 사용할 수 있다.
  ```ts
  const {css 변수명} = css`
  {css 스타일
  ...}
  `
  ```
  로 사용한다.

**Before**

```ts
export const HeadingKrRegular4 = styled.span`
  font-family: "Hyundai Sans Head KR Regular";
  font-size: 20px;
  line-height: 24px;
  letter-spacing: -0.003em;
  text-align: left;
`;
```

**After**

```ts
export const HeadingKrRegular4 = css`
  font-family: "Hyundai Sans Head KR Regular";
  font-size: 20px;
  line-height: 24px;
  letter-spacing: -0.003em;
  text-align: left;
`;
```

span 태그로 생성하는 것이 아닌 css 속성을 변수화 하는것으로 변경했다. 이유는 span 태그로 사용할 경우 변경하고 싶으면 상속받은 후, 재사용에 불편함이 있기 때문이다.

**예시**

```ts
# 예시
import styled, {css} from "styled-components";

const flexCenter = css`
  display: flex;
  justify-content: center;
  align-items: center;
`;

const FlexBox = div`
  ${flexCenter}
`;
```

검색해보니 위와 같이 자주 쓰는 css를 변수화해서 많이 사용한다고 한다. (새로운 지식 +1)
