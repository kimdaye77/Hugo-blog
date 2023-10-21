---
title: "[현대 소프티어] FE 뉴스스탠드 #1"
date: 2023-07-17T18:47:22+09:00
draft: false
categories: "현대 소프티어"
slug: "fe-newsstand1"
tags: ["프론트엔드", "newsstand", "현대소프티어"]
---

## FE 뉴스스탠드 3주차

참고 {{< icon "github" >}}
<br>
[fe-newsstand #3](https://github.com/softeerbootcamp-2nd/fe-newsstand/pull/80)
<br>
[fe-newsstand 7.17](https://github.com/kimdaye77/fe-newsstand/pull/4)
<br>
~~3주차부터 블로그 시작하는 것이라 제목은 #1이다.~~

### 프로젝트 설명

네이버 뉴스스탠드와 비슷한 기능을 하는 뉴스스탠드를 만드는 프로젝트다.
총 4주 기간동안 진행한다. 지금까지 2주가 지났고 오늘 3주차를 시작한다.
현재까지 구현한 기능은 다음과 같다.

#### 프로젝트 폴더 구조 트리

```
fe-newsstand
├─ .DS_Store
├─ assets
│  ├─ icons
│  └─ images
│     └─ logo
│        ├─ dark
│        └─ light
├─ css
│  ├─ color.css
│  ├─ index.css
│  └─ reset.css
├─ data
│  ├─ news.json
│  ├─ press.json
│  └─ recentNews.json
├─ index.html
├─ js
│  ├─ app.js
│  ├─ constants
│  │  └─ constants.js
│  ├─ sections
│  │  ├─ header.js
│  │  ├─ mainView.js
│  │  └─ recentNews.js
│  └─ utils
│     ├─ autoRolling.js
│     ├─ changeView.js
│     ├─ checkPage.js
│     ├─ getDate.js
│     ├─ makeGridView.js
│     ├─ makeListView.js
│     └─ reload.js
└─ readme.md

```

### Feature list

#### 개발 완료한 기능

```
[기본 화면]
- [x] 메인 로고 클릭 시 새로고침
- [x] 시스템 날짜 표현

[최신 뉴스 영역]
- [x] 최신 뉴스 5초마다 5개 자동 롤링
- [x] 좌우 영역 1초 차이 롤링
- [x] 마우스 호버 시 롤링 일시정지

[그리드 뷰]
- [x] 새로고침 시 언론사 로고 랜덤 표시
- [x] 첫 페이지, 마지막 페이지 버튼 disable
- [x] 뷰 방식 변경
- [x] 마우스 호버 시 구독하기 버튼 표시

[리스트 뷰]
- [x] 클릭한 카테고리의 현재 언론사 순서와 총 언론사 수 표시
- [x] 버튼 클릭 시 해당 카테고리의 다음 언론사로 넘어가기
- [x] 20s간 프로그레스 바 진행 후, 다음 언론사로 넘어가기

```

#### 남은 기능

(오늘 구현한 기능은 형광펜 표시)

- [x] <mark>구독하기/해지하기 버튼</mark> {{< icon "check" >}}
- [ ] 리스트 뷰 메인 로고 썸네일에 마우스 호버 시 5% 확대
- [x] <mark>구독한 언론사 탭 버튼 이벤트</mark> {{< icon "check" >}}
- [ ] 구독한 언론사 페이지 구현 <- 구현하다가 개발 시간이 끝났다.
- [ ] 그리드 뷰 마지막 페이지 언론사 로고 갯수 96개 아닐 때 그리드 표시
- [ ] 다크모드 (추가 미션)

### 개발한 기능 시연

![개발한 페이지](img/fe-newsstand1.png)
원래 카테고리 바에 프로그레스 바 진행이 나타나야 하는데 개발 도중에 끝나서 캡쳐를 못했다.

## 어려웠던 점

### 문제 상황

이벤트 위임을 적용하고 싶어서 리스트뷰전체에서 이벤트를 받았다.
리스트뷰에서 언론사 구독하기 클릭이벤트를 등록할 때 페이지가 증가할수록 그 수만큼 이벤트가 중첩됐다.

### 시행착오 #1

removeEventListener를 통해 이벤트를 제거하려고도 해봤으나 제거하면 클릭을 한번밖에 못함

### 시행착오 #2

각 언론사 뉴스 정보를 세분화해서 그려주는 `drawPressInfo.js` 파일에서 각각의 구독하기 버튼에 제한해서 클릭이벤트를 적용

### 해결 코드

```
{{<highlight js
"lineos=table, hl_lines = 26-29" >}}

# pressNewsInfo.js 이벤트 리스너 등록한 부분

export function drawPressInfo(category_news, subscribedPress, press) {
  const press_news = document.querySelector(".press-news");
  press_news.innerHTML = `<div class="press-info">
      <img
        id="press-logo"
        alt="press-logo"
        src="${category_news.src}"
      />
      <span class="edit-date">${category_news.edit_date} 편집</span>
      <div class="sub">
        <button class="sub subscribe">
          <img src="../assets/icons/plus.svg" />
          <span>구독하기</span>
        </button>
        <button class="sub cancel">
          <img src="../assets/icons/closed.svg" />
        </button>
      </div>
    </div>`;
  showSubscribeButton(subscribedPress, press);
  const newDiv = document.createElement("div");
  newDiv.classList.add("news-content");
  press_news.appendChild(newDiv);
  const sub_btn = document.querySelector(".press-info .sub");
  sub_btn.addEventListener("click", (e) =>
    handleClick(e, subscribedPress, press)
  );
}
{{</highlight>}}
```

언론사 정보를 그려주는 함수에서 버튼에 클릭 이벤트를 등록했다. 버튼이 클릭되면 아래의 함수가 실행된다. 아래의 함수는 버튼이 구독한 상태에서 누른 버튼인지, 아닌 상태에서 누른 버튼인지를 해당 버튼의 클래스로 조건문을 판별한다. cancel이 클래스에 포함되어 있을 경우, 현재 리스트 뷰의 버튼은 해지하기 버튼이므로 구독한 상태를 의미한다. 따라서 해당 버튼이 눌렸을 때에는 해지 처리를 아닌 경우에는 구독 처리를 해주었다.

```
{{<highlight js
"lineos=table" >}}

# pressNewsInfo.js 클릭 이벤트가 실행시켜주는 함수

function handleClick(e, subscribedPress, press) {
    const btn_target = e.target.closest("button");
    const press_news = document.querySelector(".press-news");
    const newDiv = document.createElement("div");
    if (btn_target) {
    console.log(subscribedPress, subscribedPress.includes(press));
    if (btn_target.classList.contains("cancel")) {
        console.log("해지");
        newDiv.classList.add("popup", "alert");
        newDiv.innerHTML = `
            <div class="message"><span class="press">${press}</span>을(를)\n구독해지하시겠습니까?</div>
            <div class="buttons">
                <button class="btn-yes">예, 해지합니다</button>
                <button class="btn-no">아니오</button>
            </div>`;
        press_news.appendChild(newDiv);
        const btn = document.querySelector(".buttons");
        btn.addEventListener("click", (e) =>
        checkAnswer(e, subscribedPress, press)
    );
    } else {
        console.log("현재 언론사", press);
        subscribedPress = [...subscribedPress, press];
        newDiv.classList.add("popup", "snackbar");
        newDiv.textContent = "내가 구독한 언론사에 추가되었습니다.";
        press_news.appendChild(newDiv);
        showSubscribeButton(subscribedPress, press);
        parentCallback(subscribedPress);
        newDiv.addEventListener("animationend", handleAnimationEnd);
    }}}
{{</highlight>}}
```

~~{{< icon "triangle-exclamation" >}} 아직 리팩토링 전이라 매우 코드가 보기 힘들다. console.log도 안 지운 코드 (...)~~

## 향후 계획

- 코드 리뷰하면서 같은 팀원 분 중 한 분이 고수신데 store.js, 옵저버 패턴을 사용했다. 지금 건드렸다간 버그 투성이일 것 같아서 못했지만 기능 구현을 완료하고 리팩토링을 할 때 한 번 사용하고 싶다.
- 내일은 내가 구독한 언론사 탭을 구현해야 한다. 이 탭은 기본 틀은 카테고리 별 뉴스와 같아서 카테고리를 그려주는 함수의 파라미터를 손보다가 개발 시간이 끝나서 현재 지금 안 돌아가는 상태로 존재한다. ㅎㅎ 내일은 버그를 고쳐야겠다.
