---
title: "[현대 소프티어] FE 뉴스스탠드 #7"
date: 2023-07-24T18:03:15+09:00
draft: false
slug: "fe-newsstand7"
tags: ["프론트엔드", "newsstand", "현대소프티어"]
---

## FE 뉴스스탠드 4주차

참고 {{< icon "github" >}}
<br>
[fe-newsstand #4](https://github.com/softeerbootcamp-2nd/fe-newsstand/pull/104)
<br>
[fe-newsstand 7.24 그리드뷰 구독 클릭 이벤트](https://github.com/kimdaye77/fe-newsstand/pull/10)
<br>
[fe-newsstand 7.24 다크모드 구현](https://github.com/kimdaye77/fe-newsstand/pull/11)
<br>
[fe-newsstand 7.24 구독한 언론사 드래그 기능 구현](https://github.com/kimdaye77/fe-newsstand/pull/12)

### 구현한 기능

- [x] 리스트 뷰 메인 로고 썸네일에 마우스 호버 시 5% 확대
- [x] 다크모드 (추가 미션)
- [x] 카테고리 추가영역 노출 처리(선택)
- [x] 리스트뷰 뉴스 제목 호버 시 밑줄 표시

### To do list

- [ ] 언론사 글자 수가 길 때 다음 줄로 넘어가는 문제 (알럿)
- [ ] 리스트뷰 랜덤 순서 구현
- [ ] 오버플로우 뒤에 위치한 언론사 위치 조정
- [ ] 리팩토링

---

### 시연 영상

<video controls>
  <source src="
https://user-images.githubusercontent.com/63107805/255542878-f33b3726-15db-4a08-9c3f-0ebf864aeb53.mov" type="video/mp4" />
</video>

~~시연 영상 찍다가 버그 발견했다. 구독한 언론사 탭 부분 이미지는 확대가 호버 시 안 되는 것 같다.~~

### 어려웠던 점

오늘은 이벤트를 주로 다뤄서 이벤트 관련 부분에서 어려움을 많이 겪었다. (7/24)

#### 1. 스낵바 애니메이션 종료 후, 2번 실행되는 문제

- css 애니메이션이 `fade-in`, `fade-out` 2번 적용되기 때문에 2번 실행되는 것이었다. 디버깅 때 찾느라 시간이 많이 걸렸다. 찾고나서는 간단하게 `animationEnd`이벤트 리스너로 `event.animationName`이 `fade-out`일 때에만 구독한 언론사로 넘어가게 구현했다.

#### 2. 해지하기 이벤트 리스너가 한번만 실행됨

- 개발자 도구를 이용해서 html 소스를 뜯어보니 보면 이전 `popup(snackbar, alert)`이 계속 겹쳐진 것이었다. 따라서 클릭을 해도 이벤트가 등록되지 않은 버튼이 눌려서 버튼 클릭 이벤트가 실행되지 않았다. 일단 `document.querySelector("popup")`을 통해 있다면 `removeChild`를 통해 제거했으나 같은 팀원분께서 `display.none`을 통한 것이 cost가 절감될 것이라고 해서 리팩토링 때 할 예정이다.

#### 3. mouse up, click 구분

- 오늘 구현한 기능 중 구독한 언론사가 많으면 드래그를 통해 스크롤 할 수 있게 하는 기능이 있다. 여기서 드래그가 끝날 때의 up을 할 때 click 이벤트도 같이 실행되었다.

```js
tabContainer.addEventListener("mousedown", (e) => {
  isDragging = true;
  startScrollX = e.pageX + tabContainer.scrollLeft;
  tabContent.style.cursor = "grabbing";
  startClickX = e.pageX;
});

window.addEventListener("mousemove", (e) => {
  if (!isDragging) return;
  e.preventDefault();
  const scrollX = startScrollX - e.pageX;
  tabContainer.scrollLeft = scrollX;
});

window.addEventListener("mouseup", (e) => {
  isDragging = false;
  tabContent.style.cursor = "grab";
  if (startClickX === e.pageX) {
    handleClick(e);
  }
});
```

`startClickX`는 드래그 시작 시의 X 좌표 값을 기억하는 변수로, 나중에 클릭 이벤트와 드래그 이벤트를 구분하는데 사용된다.
`if (startClickX === e.pageX) { ... }`는 드래그와 클릭 이벤트를 구분하기 위해 드래그 이벤트가 발생하기 전과 마우스를 누르고 뗀 지점의 X 좌표를 비교한다. 만약 드래그 없이 마우스를 클릭했다 떼었다면, handleClick(e) 함수를 호출한다.

++ 드래그 이벤트도 따로 빼면 좋을 것 같다.
