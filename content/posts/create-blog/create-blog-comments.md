---
title: "Hugo 블로그 제작기 - 댓글편"
date: 2023-08-04T20:06:36+09:00
draft: false
slug: "create-blog-comments"
tags: ["블로그 제작기"]
---

## Hugo 댓글 제작

### 댓글 기능 제작 결심

크롱님께서 쉬는 시간을 주셔서 오늘 고시원으로 간소한 이사(?)를 하기 때문에 힘들 것 같아서 블로그 글을 미리 쓰려고 했다.
<br>
어떠한 계기였는지는 모르겠지만 댓글 기능에 대해 생각하다가 다시 테마 설명을 보게 되었다.
<br>
처음 블로그 제작할 때에도 느꼈으나 본인이 사용하는 테마의 docs를 정독하자. 정말 친절하다 ^v^ bb<br>
테마 설명을 보니 정말 간단했다. `params.toml`에서 `showComments`의 값을 기본 값인 `false`에서 `true`로 설정하고(각 포스팅마다 댓글 표시 여부를 다르게 하고 싶으면 전체 매개 면수 말고 각 포스팅마다 설정해주면 된다.)<br>
이후, `layouts` 폴더 아래에 `partials/comments.html`을 제작하고 여기 부분은 [Hugo Docs](https://gohugo.io/content-management/comments/)를 참고하라고 되어있었다.
<br><br>
⚠️ **각 테마마다 폴더 구조가 다르므로 본인의 테마 문서를 꼭 확인할 것**

#### Disqus

`Hugo Docs`에서는 `disqus` 사용을 권장하나, 여러 글을 봤을 때 `disqus`보다는 `utterances`가 여러모로 좋은 것 같았다.<br>

1. `disqus`는 무료 라이센스로 사용하는 경우 광고가 생긴다. (이게 내가 생각하는 가장 큰 단점 같다. 광고 하나도 아니고 ux를 정말 해치는 다수의 광고가 보여진다....)
2. `disqus`보다 `utterances`를 개발자들이 많이 사용한다.
3. `disqus`보다 `utterances`가 가볍다.
   ++ 이후 알게된 것인데 이슈랑 연결되어 있어서 댓글이 달리면 알림도 온다. 근데 댓글을 다려면 깃허브 로그인을 해야 한다.

### Utterances를 사용한 댓글 기능 구현

#### 과정

![과정1](img/create-blog-comments1.png) ![과정2](img/create-blog-comments2.png)
나는 새로 레포지토리를 만들었다. <br>
[댓글용 레포지토리](https://github.com/kimdaye77/Blog-comments)
<br> 주의할 점은 Public으로 해야 한다.
(++ 나중에 배포하고 안 보여서 뭐지?? 했는데 배포하는 페이지 github.io와 블로그 포스트용 레포지토리에도 설치를 해야한다!!)

사용할 레포지토리에 utterances 설치가 끝나면 (https://utteranc.es/)에서 설정을 다하면 스크립트를 복사해서 자신의 코드에 붙여넣기를 하면 된다.

#### 다크 모드 여부에 따라 theme 설정 변경

이건 gpt와 나의 합작이다. Hugo 자체가 한글 레퍼런스가 많이 없기도 하고, 다들 다크 모드 구현은 안하고 하나의 theme만 설정했다.
<br>하지만 나는 주위에 다크모드에 예민한 사람들이 많아서 댓글 theme에 다크모드 기능을 부여하고 싶었다.

### 완성

#### 코드

```html
<script
  src="https://utteranc.es/client.js"
  repo="kimdaye77/Blog-comments"
  issue-term="pathname"
  theme="github-light"
  crossorigin="anonymous"
  async
></script>

<script>
  const currentAppearance = localStorage.getItem("appearance");
  const regex = /^Switch.*appearance$/;

  document.addEventListener("click", function (event) {
    const targetButton = event.target.closest("div");
    if (targetButton && regex.test(targetButton.title)) reloadUtterances();
  });

  // Utterances 다시 로드 함수
  function reloadUtterances() {
    const utterancesContainer = document.querySelector(".utterances");
    utterancesContainer.innerHTML = ""; // 기존 Utterances 컨테이너 비우기

    // 새로운 스크립트 엘리먼트 생성
    const newScript = document.createElement("script");
    newScript.src = "https://utteranc.es/client.js";
    newScript.setAttribute("repo", "kimdaye77/Blog-comments");
    newScript.setAttribute("issue-term", "pathname");
    newScript.setAttribute(
      "theme",
      localStorage.getItem("appearance") === "dark"
        ? "github-dark"
        : "github-light"
    );
    newScript.setAttribute("crossorigin", "anonymous");
    newScript.setAttribute("async", true);
    // Utterances 컨테이너에 새로운 스크립트 엘리먼트 추가
    utterancesContainer.appendChild(newScript);
  }
  // 페이지 로드 시 localStorage 값을 확인하여 초기 테마 설정
  window.onload = function () {
    reloadUtterances();
  };
</script>
```

#### 댓글 화면

같은 팀원분한테 테스트를 부탁했다. 확인 결과 잘 작동한다.
![댓글 화면](img/create-blog-comments3.png)

++ 다크모드 기능이 잘 되는지 확인하려고 여러 번 클릭했는데 새로운 에러메시지를 발견했다.

```js
utterances.6ec01640.js:1 Uncaught (in promise)
Error: Error fetching issue via search.

Failed to load resource: the server responded with a status of 429 ()
```

gpt 덕분에 위의 에러 발생 원인을 알아냈다.

> 서버로부터 너무 많은 요청을 받아들여 처리하지 못했을 때 발생하는 오류입니다. 이 오류는 서버에서 설정한 요청 제한을 초과하면 발생합니다.

라고 한다. 이 문제는 주로 Utterances 스크립트가 사용하는 GitHub API와 관련이 있다. Utterances 스크립트는 GitHub API를 사용하여 댓글을 가져오고 게시한다. GitHub는 남용을 방지하고 공정한 사용을 보장하기 위해 API 요청에 대해 요청 제한을 설정한다. Utterances 컴포넌트를 반복적으로 로드하거나 다시 로드하면 요청 제한을 초과하여 429 오류가 발생할 수 있다고 한다.

### 향후 계획

요청 제한도 기능 구현해야겠다.
자동 배포랑 도메인 적용도 해야하는데.. 언제하지
