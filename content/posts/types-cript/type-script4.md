---
title: "[TS] Type script 공부 #4"
date: 2023-07-30T20:53:41+09:00
draft: false
categories: "타입스크립트"
slug: "type-script4"
tags: ["타입스크립트", "리액트", "노마드코더"]
---

## Type script 공부 #4

[노마드코더 강의 링크](https://nomadcoders.co/react-masterclass/lobby)
<br>
오늘도 이어서 하는 타입스크립트 클론 코딩! 🐜

👇🏻 아래는 클론코딩용 깃허브 레포지토리 링크이다. 👇🏻<br>
[깃허브 링크](https://github.com/kimdaye77/crypto-tracker)

오늘 완료한 기능 구현은 다음과 같다.

- coin 상세 화면 구현
- coin 상세 화면 내 탭 구현

### API interface

[[TS] Type script 공부 #3](https://kimdaye77.github.io/posts/types-cript/type-script3/#type-%EC%A0%95%EB%B3%B4-%EC%83%9D%EC%84%B1) 여기서 API 인터페이스를 작성할 때 중요한 점이 있다. 가장 중요한 단점이 있기 때문이다.
`Object.values(temp1).map(v => typeof v).join()`으로 타입을 얻을 경우에는, array가 있으면 object로 결과가 나오기 때문에 직접 설명해줘야 한다.
따라서 타입이 object로 나오는 key들은 아래와 같이 상세 보기를 눌러줘서 복사+붙여넣기를 해서 수정해준다.
![타입상세](img/typescript4-1.png)
interface ITag {
coin_counter: number;
ico_counter: number;
id: string;
name: string;
}

수정을 하면 다음과 같다.

```ts
# Coin.tsx

// 각 object를 설명해주는 인터페이스 추가
interface ITag {
  coin_counter: number;
  ico_counter: number;
  id: string;
  name: string;
}

interface InfoData {
    // object를 해당하는 인터페이스로 수정
    tags: ITag[];
}

// 설명 완료된 api 인터페이스를 적용
const [info, setInfo] = useState<InfoData>();
const [priceInfo, setPriceInfo] = useState<PriceData>();

```

#### 예외

이전 포스팅에서 아래와 같이 말했다.

```
하지만, 아래의 상황에서 문제가 발생할 가능성이 있다.
1. 시크릿창일 때 링크의 데이터를 못 받는다.
2. 링크로 다이렉트로 검색할 때
위의 예외 처리를 해주어야 한다.
```

이 경우에는,

```tsx
<Title>
  코인 {state?.name ? state.name : loading ? "Loading..." : info?.name}
</Title>
```

1. state가 없으면(이전 링크에서 데이터를 받지 않았다는 의미) 로딩 상태 확인
2. 로딩 끝나면 info.name을 띄어준다.

예외처리를 이렇게 해준다.

### useEffect

리액트에는 여러가지 사용할 수 있는 Hook들이 있다. useEffect는 그 중 한가지이다.
컴포넌트가 렌더링 될 때 특정 작업을 실행할 수 있도록 하는 Hook이다

```tsx
useEffect(() => {
  // 실행 함수
}, []);
```

첫번째 인자(effect)는 함수, 두번째 인자는 배열(deps)이다.

#### deps

- 빈 배열을 입력할 경우 컴포넌트가 렌더링 될 때에만 실행된다.
  즉, []은 **no dependency**를 의미한다.
- 특정한 값이 변경될 때 effect 함수를 실행하고 싶을 경우 배열 안에 그 값을 넣어준다.
  만약, [x] 넣었을 경우 x의 상태가 변화면 다시 실행된다.

이 때, 리액트는 Hook 안에서 사용하는 변수는 dependency 넣는 것을 권장한다.

### useRouteMatch

또 다른 Hook이다. `useRouteMatch`는 현재 위치를 기준으로 지정된 경로에 대한 일치 데이터를 반환한다. 즉, 현재 특정한 url에 있는지 여부를 알려준다.<br>
(v6에서는 `useMatch`로 써야한다. ~~++ 참고로 Switch -> Routes~~)

#### matchPath()

url 경로 이름에 대해 경로 패턴을 일치시키고 일치에 대한 정보를 반환한다.

### nested router

route 안에있는 또 다른 라우트 렌더링 가능하게 해준다.

1. 웹사이트에서 탭을 사용할 때
2. 스크린 안에 많은 섹션이 나눠진 곳
   을 구현할 때 아주 유용하다.

#### 탭 구현

위에서 설명한 `useRouteMatch`와 `nested router`를 사용해서 탭을 구현할 수 있다. url..말고도 state로도 할 수 있지만 url이 사용성이 높다. 사용자가 바로 접속할 수 있기 때문이다.

```tsx
# Coin.tsx

// Tab의 스타일 컴포넌트
const Tab = styled.span<{ isActive: boolean }>`
  text-align: center;
  text-transform: uppercase;
  font-size: 12px;
  font-weight: 400;
  background-color: rgba(0, 0, 0, 0.5);
  padding: 7px 0px;
  border-radius: 10px;
  color: ${(props) =>
      props.isActive ? props.theme.accentColor : props.theme.textColor} a {
    display: block;
  }
`;

// 스타일 컴포넌트에게 isActive 값을 넘겨준다.
<Tab isActive={chartMatch !== null}>
    <Link to={`/${coinId}/chart`}>Chart</Link>
</Tab>
<Tab isActive={priceMatch !== null}>
    <Link to={`/${coinId}/price`}>Price</   Link>
</Tab>

const priceMatch = useRouteMatch("/:coinId/price");
const chartMatch = useRouteMatch("./coinId/chart");
```

#### 구현 화면

탭을 누르면 누른 탭이 활성화된다.
![구현 화면](img/typescript4-2.png)
