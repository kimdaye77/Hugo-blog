---
title: "[Project] Cartag 프로젝트 #4"
date: 2023-08-13T21:59:05+09:00
draft: false
slug: "프로젝트4"
tags: ["타입스크립트", "리액트", "프로젝트"]
---

## Cartag 프로젝트 #4

오늘은 주말에도 팀원 분들과 모여서 프로젝트를 진행했다. 개발한 기능으로는 여러 상세 애니메이션과 추가 기능이다. 추가 기능으로 예산 설정바에 **드래그 앤 드롭** 기능을 넣었다. 마우스 커서를 그대로 있게 하느라 시간을 많이 썼다.

### 구현 영상

<video controls>
  <source src="
https://github.com/softeerbootcamp-2nd/A2-CarTag/assets/63107805/dcc4e9dd-5372-4beb-b927-899424aa5a4d
" type="video/mp4" />
</video>

### 코드 설명

```tsx
export default function PriceStaticBar({ ...props }: IPriceStaticBar) {
  const [startX, setStartX] = useState(0);
  const [startY, setStartY] = useState(0);
  const [offset, setOffset] = useState({ offsetX: "50%", offsetY: "76px" });
  const barRef = useRef<HTMLDivElement>(null);


  useEffect(() => {
    window.addEventListener("mousemove", handleMouseMove);
  });


  const handleMouseDown = (event: React.MouseEvent) => {
    dragRef.current = true;
    const element = barRef.current!.getBoundingClientRect();
    setStartX(event.clientX - element.left);
    setStartY(event.clientY - element.top);
  };

  const handleMouseMove = (event: MouseEvent) => {
    if (!dragRef.current) {
      window.removeEventListener("mousemove", handleMouseMove);
      return;
    }

    const minX = 0;
    const minY = 60;
    const maxX = window.innerWidth - (barRef.current?.offsetWidth || 0);
    const maxY = window.innerHeight - (barRef.current?.offsetHeight || 0);
    const newLeft = Math.min(Math.max(minX, event.clientX - startX), maxX);
    const newTop = Math.min(Math.max(minY, event.clientY - startY), maxY);

    setOffset({ offsetX: `${newLeft}px`, offsetY: `${newTop}px` });
  };

  const handleMouseUp = () => {
    dragRef.current = false;
  };
```

#### handleMouseDown

우선, `handleDown`은 커서로 예산 설정바를 잡을 때 실행되는 이벤트 핸들러이다. `dragRef`는 마우스가 드래그 이벤트를 실행해야 하는지를 알려준다. `useState`를 사용할 경우 렌더링 시에 값이 바껴서 false로 초기화가 돼서 `useRef`를 사용했다. dragRef.current 값을 true로 바꿔준다.<br>
또한 나중에 마우스 커서 위치를 기억하기 위해 startX, startY에다 각각의 statusBar 내에서의 위치를 기억해준다. `barRef`는 예산 설정바인데, `barRef.current!.getBoundingClientRect();`이렇게 해주면 그 요소까지의 위치 정보를 반환해준다.

#### handleMouseMove

dragRef.current가 true일 때에만 드래그 이벤트를 실행해준다. minX, minY, maxX, maxY는 화면에서 예산 설정바가 벗어나지 말아야 할 최솟값 범위이다. 이 때 마우스커서 위치를 그대로 해주기 위해 newLeft, newTop에 `handleMouseDown`에서 설정한 start 값을 각각 빼준다. (안 하면 점프 현상이 일어난다.)

#### handleMouseUp

마우스 커서를 뗐다는 소리이다. (드롭과 의미 같음) 이 때에는 드래그 이벤트를 끝내야 하므로 `dragRef.current`값을 false로 설정해주면 된다.

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

예산 설정바 스타일 컴포넌트에서 top, left 값을 state로 변경해주면 적용된다.

### 이후 할 것

마우스 드래그 이벤트를 `window`로 줘서 그런가 드래그가 그 요소에도 된다. `dragRef.current`가 `true`일 경우에는 드래그를 방지할 수 있나 찾아 봐야겠다.
