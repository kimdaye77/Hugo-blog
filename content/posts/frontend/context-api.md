---
title: "[FE] Context API"
date: 2023-08-11T20:30:14+09:00
draft: false
slug: "context-api"
tags: ["프론트엔드", "리액트"]
---

## Context API

오늘은 `Context API`라는 주제에 대해 다뤄보겠다. 이번 프로젝트 Cartag에서 Redux 사용이 금지되어서 대신 사용하는 기술이다.(엄밀히 말하면 둘은 다른 목적을 가진 기술이다.) 이번 기회를 통해 알게 되었다.

### Context API란?

우선, Context API는 쉽게 말하면 전역 상태이다. 리액트에서는 상태 관리를 하기 위해서 `useState` 훅을 사용한다. 이 상태는 사용한 컴포넌트 내에서만 사용 가능하고 다른 컴포넌트에게 전달을 하기 위해서는 `props`를 통해 전달을 해주어야 하는데, 이는 보통 **하향식**으로 일어난다. 이러한 특성 탓에 한 단계 아래의 자식이 아니라, 몇 단계를 뛰어 넘는 자식에게 속성 값을 전달하고 싶거나 얽히고 섥힌 관계가 있을 경우... **props drilling 문제**가 생기고 복잡해진다!
![context api](https://miro.medium.com/v2/resize:fit:2000/1*Ha2vNB0ILaYKPXk6oyTZSQ.png)

### Props Drilling

이름에서부터와 리액트로 개발을 해 본 사람들은 `Props drilling`이라는 단어에서부터 무슨 의미인지 직감할 것이다.
이러한 Props drilling 문제를 해결하기 위한 기술이 지금 기술하는 Context API이다.<br>
위애서 서술했듯이 props drilling 하위 컴포넌트에게 전달해주기 위해 props를 계속 아래로 전달해줘야 하는 문제이다.

#### 코드 예시

```js
export default function App() {
  return (
    <div className="App">
      <FirstComponent content="Who needs me?" />
    </div>
  );
}

function FirstComponent({ content }) {
  return (
    <div>
      <h3>I am the first component</h3>;
      <SecondComponent content={content} />|
    </div>
  );
}

function SecondComponent({ content }) {
  return (
    <div>
      <h3>I am the third component</h3>;
      <ComponentNeedingProps content={content} />
    </div>
  );
}

function ComponentNeedingProps({ content }) {
  return <h3>{content}</h3>;
}
```

위와 같이 몇 단계 아래의 컴포넌트에게 content라는 값을 전달해주고 싶으면 props를 순차적으로 자식에게 전달해야 한다..

```js
export default function App() {
const content = "Who needs me?";
return (

<div className="App">
    <FirstComponent>
        <SecondComponent>
            <ComponentNeedingProps content={content}  />
        </SecondComponent>
    </FirstComponent>
</div>
);
}

function FirstComponent({ children }) {
    return (
        <div>
        <h3>I am the first component</h3>;
        {children}
        </div>
    );
}

function SecondComponent({ children }) {
    return (
        <div>
        <h3>I am the third component</h3>
        {children}
        </div>
    );
}

function ComponentNeedingProps({ content }) {
    return <h3>{content}</h3>
```

원래라면 props를 계속 전달해야 했지만 Context API를 쓰면 전역 상태처럼 Context Tree는 컨텍스트 안에 포함된 모든 레벨에서 명시적으로 prop 을 전달하지 않고, 어디서든 상태값에 접근 할 수 있는 방법을 제공한다. 따라서 `prop-passing` 로직을 작성할 필요가 없기 때문에 코드가 단순해진다.

### Context API vs Redux

나는 처음에 `Redux`와 개념이 헷갈렸다. 하지만 두 기술은 목적이 다르다. Context API만 검색해도 연관 검색어에 vs Redux가 나온 만큼 두 기술 모두 전역 상태로는 둘 다 사용할 수 있다. 하지만 내가 이해한 바로는 다음과 같다.

#### Context API

Provider와 Consumer의 관계 같이 제공하고 소비하는 개념

#### Redux

- Publish Subscribe 개념도 포함
- 옵저버 패턴처럼 상태 변화를 목적에 두는 기술<br>

실제로 크롬 확장 도구에 있는 `Redux Devtool` 프로그램에서는 이러한 리덕스의 특성을 통해 디버깅 할 수 있는 것을 보아 이 부분의 특성이 매우 강한 것 같다!!
