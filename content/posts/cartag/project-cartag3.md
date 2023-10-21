---
title: "[Project] Cartag 프로젝트 #3"
date: 2023-08-03T18:49:50+09:00
draft: false
categories: "현대 소프티어"
slug: "프로젝트3"
tags: ["타입스크립트", "리액트", "프로젝트"]
---

## Cartag 프로젝트 #3

오늘은 제너럴 컴포넌트를 예산 초과 여부에 따라 박스 내의 텍스트와 색상을 바꿔주었다. 또한 설정한 예산의 마커 UI를 추가해주었다.

### 구현 영상

<video controls>
  <source src="
https://github.com/kimdaye77/Hugo-blog/assets/63107805/402b4cb1-65df-4cb5-a3f4-59e2dc0d45c4" type="video/mp4" />
</video>

### 어려웠던 점

설정한 예산 값의 현재 지점 표시하는 마커가 슬라이드 바의 핸들을 가려서 겹칠 경우, 클릭이 안먹는 버그가 있었다.
하지만 같은 팀원분께서 CSS 이벤트 제어 `pointer-events` 속성을 사용해서 `none` 값을 적용하면 마커의 클릭 이벤트를 제거할 수 있다고 알려주셔서 해결할 수 있었다.

### 시행착오

#### z-index

`z-index`를 줘봤는데 핸들이 클릭 이벤트가 가능하도록 하기 위해 높은 값을 부여하면 마커가 슬라이드 바 뒤에 가서 이 방법은 기각되었다.

#### pointer-events

팀원 분께서 알려주셔서 해결한 방법이다!<br>
`pointer-events` 는 HTML 요소들의 마우스/터치 이벤트들(CSS hover/active, JS click/tap, 커서 드래그등)의 응답을 조정할 수 있는 속성이다.

```
none : HTML 요소에 정의된 클릭, 상태(hover,active등), 커서 옵션들이 비활성화한다.
auto : 비활성화된 이벤트를 다시 기본 기능을 하도록 되돌린다.
inherit : 부모 요소로부터 pointer-events 값을 상속받는다.
```

나의 경우에는 `none` 값을 적용해서 클릭 커서 옵션을 비활성화했다.<br>
따라서 마커와 핸들이 겹쳐있더라도 마커는 클릭이 안 되도록 했기 때문에 뒤에 있는 핸들의 클릭 이벤트가 가능하게 되었다.

#### 코드

```
{{<highlight tsx
"lineos=table, hl_lines = 3">}}
# PriceStaticBar.tsx

const MarkerSvg = styled.svg<{ $isover: boolean; $percent: number }>`
  pointer-events: none;
  position: absolute;
  width: 9px;
  height: 23px;
  top: 6px;
  fill: ${(props) =>
    props.$isover ? props.theme.color.sand : props.theme.color.primaryColor400};
  left: ${(props) => props.$percent}%;
  transform: translate(-50%, -50%);
`;
{{</highlight>}}
```

또한 중요한 점은 `pointer-events` 속성은 11개의 속성값을 가지지만 3개를 제외하고는 모두 SVG에서 사용하도록 예약되어 있다. 아래의 3개의 속성값은 HTML 요소들에서 사용 가능하다.라는 것인데 마침 마커가 svg라 가능했다.

### 마치며

이후의 계획으로는 백엔드 팀원분들과 협력해서 배포를 할 계획인데 기대가 된다!
