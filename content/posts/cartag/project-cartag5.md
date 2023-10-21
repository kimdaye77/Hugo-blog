---
title: "[Project] Cartag 프로젝트 #5"
date: 2023-08-14T20:56:01+09:00
draft: false
categories: "현대 소프티어"
slug: "프로젝트5"
tags: ["타입스크립트", "리액트", "프로젝트"]
---

## Cartag 프로젝트 #5

오늘은 [[Project] Cartag 프로젝트 #4](https://kimdaye77.github.io/posts/cartag/%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B84/)에서 설명한 **드래그 앤 드롭**기능을 구현하다가 생긴 경고를 해결하면서 알아낸 것들을 정리하고자 한다.

### 뒤늦게 만난 Warning 🚨

`Over 200 classes were generated for component styled.section. Consider using the attrs method, together with a style object for frequently changed styles.`

이라는 경고가 나타난 것을 모르고 있었다가 팀원 분께서 말해서 고치려고 검색해보았다.<br>

무슨 경고냐면 아래와 같이 top, left를 동적으로 state인 offset으로 줬다. 여기서 드래그할 때 offset이 바뀌면 스타일 컴포넌트가 렌더링되면서 class가 너무 많이 변경된다고 경고하는 것이다.
이렇게 동적인 속성은 스타일 컴포넌트의 attrs를 통해 인라인 스타일로 바꾸라고 메시지에서 알려준다.

```tsx
const StatusBox = styled.div<{
  $offset: IOffset;
  $isover: boolean;
  $isopen: boolean;
}>`
  position: fixed;
  min-width: 343px;
  z-index: 1000;
  top: ${({ $offset }) => $offset.offsetY};
  left: ${({ $offset }) => $offset.offsetX};
  transform: ${({ $offset }) =>
    $offset.offsetX === "50%" ? "translateX(-50%)" : null};
  (중략)cursor: pointer;
`;
```

### 변경 후 코드 (attrs 버전)

```tsx
<StatusBox
      ref={barRef}
      $isover={isOverBudget}
      $isopen={isOpen}
      onMouseDown={handleMouseDown}
      {...props}
      $offset={offset}
    >
```

위에서 스타일 컴포넌트에 attrs 메소드를 사용해서 $offset 속성 값을 전달해준다. 스타일 컴포넌트의 attrs 메소드는 컴포넌트에 HTML 속성을 부여해준다.
위의 코드에서 $offset은 styled.div 컴포넌트의 attrs 메소드 내에서 정의되고, 해당 컴포넌트의 스타일을 설정하는 데 사용된다. 아래에서 style을 지정해주는 것을 확인할 수 있다.

```
{{<highlight tsx
"lineos=table, hl_lines =3-13">}}
# PriceStaticBar.tsx

const StatusBox = styled.div.attrs<{
  $isover: boolean;
  $isopen: boolean;
  $offset: IOffset;
}>(({ $offset }) => ({
  style: {
    left: $offset.offsetX,
    top: $offset.offsetY,
    transform: $offset.offsetX === '50%' ? 'translateX(-50%)' : 'none',
  },
}))`
  ${({ $isover }) => !$isover && withinBudgetCss}
  ${({ $isover }) => $isover && overBudgetCss}
  position: fixed;
  min-width: 343px;
  z-index: 1000;
  padding: 0px 16px;
  border-radius: 10px;
  backdrop-filter: blur(3px);
{{</highlight>}}
```

위와 같이 자주 바뀌는 offset의 경우 attrs 메소드를 이용했다.

#### 해당 PR

[[FIX] #205: 드래그 시 스타일 컴포넌트 많이 생기는 경고 해결](https://github.com/softeerbootcamp-2nd/A2-CarTag/pull/232)
