---
title: "[현대 소프티어] FE 뉴스스탠드 #9"
date: 2023-07-26T17:51:34+09:00
draft: false
categories: "현대 소프티어"
slug: "fe-newsstand9"
tags: ["프론트엔드", "newsstand", "현대소프티어"]
---

## FE 뉴스스탠드 4주차

참고 {{< icon "github" >}}
<br>
[fe-newsstand #4](https://github.com/softeerbootcamp-2nd/fe-newsstand/pull/104)
<br>
[fe-newsstand 7.26 버그 수정](https://github.com/kimdaye77/fe-newsstand/pull/15)
<br>
[fe-newsstand 7.26 리스트뷰 랜덤 구현](https://github.com/kimdaye77/fe-newsstand/pull/16)
<br>
[fe-newsstand 7.26 예외 처리](https://github.com/kimdaye77/fe-newsstand/pull/17)

### 구현한 기능

**기능 구현**

- [x] 리스트뷰 랜덤순서
- [x] 구독한 언론사 탭 누르면 리스트뷰 디폴트 설정

**예외 처리 및 버그 수정**

- [x] 25개일때 해지하고 페이지 처리
- [x] 여러 개 해지되는 현상 해결
- [x] 오버플로우 뒤에 위치한 언론사 위치 조정
- [x] 첫번째 페이지에서 이전 버튼 클릭시 마지막 페이지
- [x] 그리드뷰 버튼에만 클릭이벤트 범위 적용하기
- [x] 구독한 언론사 아무것도 없을때 -> alert 처리
- [x] 25개일때 해지하기 페이지 처리
- [x] 여러개 해지되는 현상 해결

### To do list

- [ ] 다른 곳 선택 시 스낵바 에니메이션 제거
- [ ] 리팩토링

---

### 시연 영상

<video controls>
  <source src="
https://github.com/kimdaye77/fe-newsstand/assets/63107805/e3bf5e43-a396-4009-a362-3cbbd640ad64" type="video/mp4" />
</video>

~~에휴 항상 시연 영상 찍다가 버그를 발견한다. 전에 고친 버그인데 잘못 수정하다가 다시 발생한 것 같다.~~
<br>To do. 다른 곳 선택하면 스낵바 애니메이션 종료 후 구독한 언론사 탭으로 이동하는 이벤트 제거

### 어려웠던 점

오늘은 어제 고치기로 생각한 해지가 여러번 되는 현상을 수정하느라 애먹었다. (7/26)
<br>
**참고** [해지 여러번 되는 버그 해결](https://github.com/kimdaye77/fe-newsstand/pull/15/commits/eb729d9232ef52fef3770a35796fb5a6f9af4ec5)

#### Before

```js
# subscribePress.js
export function handleSubscribe(_press) {
  const isSubscribed = getSubscribedPress().some(
    (press) => press.name === _press.name
  );
  document.addEventListener("click", (e) => checkRange(e));
  deletePopupAndAnimation();
  closeAllPopups();

  //구독한 상태에서 누를 경우
  if (isSubscribed) {
    store.setState({ currentPress: _press });
    const alert = document.querySelector(".alert");
    alert.style.display = "block";
    const press_msg = alert.querySelector(".press");
    press_msg.innerHTML = `${getCurrentPress().name}`;
    const btn = document.querySelector(".buttons");
    btn.addEventListener("click", (e) => {
      checkAnswer(e, _press);
      alert.style.display = "none";
    });
  }
  //구독하지 않았을 때 => 구독됨
  else {
    const updatedSubscribedPress = [...getSubscribedPress(), _press];
    store.setState({ subscribedPress: updatedSubscribedPress });
    const snackbar = document.querySelector(".snackbar");
    snackbar.style.display = "block";
    document.addEventListener("animationend", (e) =>
      handleAnimationEnd(e, _press)
    );

    //구독한 상태로 바뀜
    getView() === "grid" ? showGridView() : showListView();
  }}

```

원래 버그 해결 전에는 각각 `showGridView`와 `showListView`에서 구독 또는 해지 버튼을 클릭할 경우에 `subscribe.js` 파일에서 `handleSubscribe` 함수를 불러와서 실행하였다. 이렇게 실행된 `handleSubscribe`는 각각 구독인지/해지인지 판단하고 해당하는 이벤트를 등록해주었다. 이렇게하니 해지하기 버튼을 선택하지 않고 곧바로 다른 언론사의 해지하기 버튼을 누르면 이벤트가 중첩돼서 삭제가 중첩되었다. 따라서
[다음 코드]({{< ref "#after" >}})와 같이 대대적으로 함수를 이동시켰다.

#### After

```js

# MainView.js
function attachEventListner() {
  document.addEventListener("click", (e) => handleClick(e));
  document.addEventListener("animationend", (e) => handleAnimationEnd(e));
  const btn = document.querySelector(".buttons");
  btn.addEventListener("click", (e) => {
    checkAnswer(e);
    });}
```

위의 코드는 버그를 고친 후이다. 이벤트 등록을 `view` 최상위인 `MainView.js`에서 등록을 해주었다. 이렇게 하니 애니메이션 중첩 현상이 사라졌다!

### 코드리뷰

오늘은 수요일이기 때문에 크롱님께서 다른 분들의 코드를 보며 리뷰해주시는 시간을 가졌다.

1. 비동기 함수 병렬 처리
   <br>
   비동기 함수에는 `await`, `promise` 등이 있다. 비동기 함수를 쓰다보면 api 여러개 써야할 때가 있다. 이럴 때에는 `await`을 여러 번 사용하는 경우 순차적으로 실행이 다 된 후에 코드들이 진행되기 때문에 일종의 <mark>blocking</mark> 현상이 발생한다.
   <br>이럴 때에는 <mark>promise.all</mark>를 통해 해결할 수 있다. `promise.all`을 사용하고 이후 `.then`을 통해 병렬 처리 해주면 된다. 그러면 비동기 함수들을 **병렬 처리**할 수 있다.
2. Delegation
   크롱님께서 저번 시간과 이번 시간에 `Delegation`에 대해 말씀해주셔서 뭔지 궁금해서 검색해보았다.
   간단히 말하면 디자인 패턴의 일부로 어떤 `helper` 객체를 이용하여 데이터를 제공하거나 특정 작업을 수행하는 것을 위임하는 패턴을 말한다.

### 마치며

이번주까지 완성해야 했던 뉴스스탠드의 기능 완료를 끝내서 너무 뿌듯하다. 언제 완성하지 막막했던 것이 엊그제같은데 ㅠㅠ
이제 남은 것은 시연하면서 알게 된 재발생한 버그+리팩토링이다. ~~하 리팩토링 어디서부터 건들여야 할 지...~~
오늘은 짜투리 시간에 `Styled Component`도 공부해봤다. 잠깐 맛만 봤는데 신세계다.
