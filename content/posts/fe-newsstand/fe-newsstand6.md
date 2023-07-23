---
title: "[현대 소프티어] FE 뉴스스탠드 #6"
date: 2023-07-23T22:06:04+09:00
draft: false
slug: "fe-newsstand6"
tags: ["프론트엔드", "newsstand", "현대소프티어"]
---

## FE 뉴스스탠드 (주말에도 이어서 ...)

참고 {{< icon "github" >}}
<br>
[fe-newsstand](https://github.com/kimdaye77/fe-newsstand/pull/9)

### Done

오늘은 주말이지만 열심히 코딩을 달렸다. 왜냐? 곧 8월에 프로젝트를 들어가기 전에 미션은 빨리 끝내면 좋을 것 같아서... 지금 블로그 글을 쓰고 있는 와중에 너무 졸리다.
구현한 기능은 아래와 같이 할 일 목록들 중 5개 정도 구현했다.

![todo](img/fe-newsstand6.png)
오늘 구현하면서 DOM에 대해 더 알게 된 것 같다.

### DOM

우선 DOM에 대해 무엇인지 간단하게 짚고 가겠다.

#### Document Object Model

HTML 문서에 접근하기 위한 일종의 인터페이스이다. DOM은 계층적으로 정보를 표현하는 <mark>**노드 트리 구조**</mark>이다.
![DOM](img/dom.png)
위의 그림과 같이 우리가 흔히 알고있는 div, span 등은 노드이다. 여기서 text 정보 또한 노드인 것을 유념해야 한다.
html 정보들을 바꾸고 싶을 때 우리는 DOM API를 많이 사용하는데 다음과 같은 것들이 있다.

#### DOM API

```js
# 노드 선택
querySelector(), querySelectorAll()
getElementById()

# 노드 탐색
parentNode(), firstChild()

# 노드 생성 및 삭제
createElement()
appendChild()
removeChild()

# 노드 정보 변경
innerHTML, innerText;
```

DOM API는 매우 편리하지만 변경함에 따라 이벤트가 작동이 안 될수도 있다는 것을 알아야 한다...

### 코드에서 DOM API

```js
# 실행 함수

function handleEvent(event, img) {
  const li = img.parentNode;
  const button = img.nextElementSibling;
  switch (event) {
    case "over":
      img.style.display = "none";
      button.style.display = "flex";
      li.style.backgroundColor = "var(--surface-alt)";
      break;
    case "out":
      img.style.display = "block";
      button.style.display = "none";
      li.style.backgroundColor = "var(--surface-default)";
      break;
    default:
      break;
  }
}
```

```js
{{<highlight js
"lineos=table, hl_lines = 22">}}
# 이벤트 핸들러

export function showGridView() {

 (생략)

  for (
    let i = PRESS_VIEW_COUNT * (getPage() - 1);
    i < PRESS_VIEW_COUNT * getPage();
    i++
  ) {
    const isSubscribed = getSubscribedPress().some(
      (press) => press.index === list[i]
    );
    const li = document.createElement("li");
    let img = document.createElement("img");
    img.setAttribute("class", "logo-img");
    main_list_ul.appendChild(li);
    if (list.length > i) {
      img.setAttribute("src", `${PRESS_LOGO_IMG_PATH}${list[i]}.svg`);
      li.append(img);
      li.innerHTML += showSubscribeButton(isSubscribed);
      img = li.querySelector("img");
      li.addEventListener("mouseover", () => handleEvent("over", img));
      li.addEventListener("mouseout", () => handleEvent("out", img));
    } else {
      li.style.cursor = "default";
    }
  }
  }
  {{</highlight>}}
```

#### 코드 설명

위의 코드는 어려움을 겪었던 코드이다. 여기서 문제가 생겼던 부분은 img 태그를 인자로 받았을 때 `null`로 받아져서 실행 함수에서 `li`, `button` 역시 마찬가지로 `null`이 돼서 해당 노드를 찾지 못한 것이다.

문제가 발생한 이유는 하이라이트 표시한 `li.innerHTML` 부분 때문이다.

#### innerHTML

우선 `innerHTML`에 대해 알아보자.
innerHTML은 JavaScript에서 DOM 요소의 내부 HTML 콘텐츠에 접근하고 수정하는 속성이다. 이를 사용하여 요소의 자식 요소를 추가하거나 변경할 수 있다.
html을 변경할 수 있기 때문에 템플릿 리터럴과 병행해서 사용하면 class, id 등 속성도 줄 수 있고 굉장히 편리하다.
<br><br>
**하지만!**

> innerHTML을 사용할 때 주의해야 할 점은, 이 속성을 사용하면 기존의 내부 콘텐츠가 완전히 대체되기 때문에 해당 요소 안에 있던 이벤트 핸들러들이나 다른 DOM 요소들은 모두 사라지게 됩니다. 따라서 innerHTML을 사용하여 DOM을 변경할 때에는 주의하여 기존의 요소들이 어떻게 영향을 받는지를 고려해야 합니다.

라고 gpt도 주의시킨다.

#### 문제 발생 이유 및 해결 방법

innerHTML을 사용하면 이전에 생성한 요소를 포함하는 요소 `innerHTML`내부의 전체 콘텐츠가 다시 생성된다.
<br>따라서 innerHTML 기본적으로 기존 요소가 제거되고 반환된 콘텐츠로 대체된다.
`li.innerHTML += showSubscribeButton(isSubscribed);`
결과적으로 이전에 생성된 `img`요소는 더 이상 `li`요소에 존재하지 않는다. 따라서 다시 `img = li.querySelector("img");`를 통해 해당 노드를 찾아주었다.
