---
title: "[FE] 리액트 훅 - useRef"
date: 2023-08-10T20:29:13+09:00
draft: false
categories: "프론트엔드"
slug: "useRef"
tags: ["프론트엔드", "리액트훅"]
---

## useRef

오늘은 `useRef`라는 리액트 훅에 대해서 알아보겠다. 저번 **FE-NEWSSTAND** 프로젝트에서는 DOM API를 많이 사용했었다. DOM API로는 `querySelector`, `getElementById`등이 있다. 해당하는 api를 써서 찾고 싶은 클래스 이름이나 아이디를 써서 html 요소를 바꿀 수 있었다.<br>
하지만 이번 **Cartag** 프로젝트는 `styled-components`를 사용하기 때문에 클래스 이름이 브라우저가 렌더링될 때 랜덤으로 지정된다.(중복 방지)<br>
따라서 DOM API를 사용하기 힘든데 이를 위해 리액트는 `useRef`라는 훅 기능을 지원해준다.
이를 통해 특정 엘리먼트의 크기, 스크롤바 위치 등 다양한 것들을 설정할 수 있다.
<br>
우선 프로젝트에서 사용한 예시로 자세히 알아보겠다.

### 실제 적용 예시

#### 코드 #1

```tsx
import { useRef } from "react";

const tabDivisionRef = useRef<HTMLDivElement>(null);
useEffect(() => {
  if (tabDivisionRef.current) {
    const tabDivisionWidth = tabDivisionRef.current.offsetWidth;
    setTabDivisionWidth(tabDivisionWidth);
  }
}, [tabDivisionRef]);
return (
  <TabWrapperInner ref={tabDivisionRef}>
    <Tab $offset={page * -tabDivisionWidth}>(중략)</Tab>
  </TabWrapperInner>
);
```

우선, 위와 같이 다른 리액트 훅처럼 import를 해준다. 그리고 변수에다가 useRef를 통해 ref 객체를 만든다.
이후 return문에서 지정할 DOM을 찾아(스타일 컴포넌트) ref 객체에 설정해준다.
{ref 객체}.current값은 우리가 원하는 DOM을 가리키게 된다!!<br>
위의 예시는 `tabDivisionRef`에 저장한 ref 객체의 offsetWidth값(엘리먼트의 width)이 바뀔 때마다 `setTabDivisionWidth(tabDivisionWidth)` 코드로 상태를 업데이트 하는 코드이다.

#### 코드 #2

```tsx
const hmgDataBgRef = useRef<HTMLDivElement>(null);

const handleClick = (event: MouseEvent) => {
  if (guideBubbleRef.current?.contains(event.target as Node)) {
    return;
  }
  if (hmgDataBgRef.current?.contains(event.target as Node)) {
    return;
  }
  setDisplayDimmed(false);
};
```

이번에는 {ref 객체}.current.contains()를 통해 hmgDataBgRef로 DOM 요소를 참조하고 클릭 이벤트가 발생했을 때 해당 요소의 안에 있는지 여부를 확인하여 setDisplayDimmed 상태를 변경하는 예시이다.

### 이외의 다른 사용 예시

#### useRef 안에 변수 넣기

```
const renderingCount = useRef(1);
```

`useState`와 달리 `useRef`로 관리하는 값은 값이 변해도 화면이 렌더링되지 않는다. 따라서 이러한 특성을 이용해서 컴포넌트 렌더링 횟수 등을 알 수 있다고 한다.

### 개인적인 소견

위처럼 유용한 기능이 많지만 이때까지 DOM API를 많이 사용해서 그런가 약간 불편하다. 어떤 점이 불편하냐면 useRef는 한 컴포넌트 내에서는 DOM 관리에 무척이나 용이하지만 다른 컴포넌트의 DOM을 가져오는 건 익숙하지 않아서 불편하다. 하지만 유용하다는 점은 틀림없고, 지금과 같이 스타일 컴포넌트를 사용하는 경우에는 useRef는 없어서는 안되는 중요한 훅인 것은 분명하다.
