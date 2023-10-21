---
title: "[FE] CSS 라이브러리와 프레임워크"
date: 2023-08-06T22:13:26+09:00
draft: false
categories: "프론트엔드"
slug: "css"
tags: ["프론트엔드"]
---

## CSS

오늘은 무엇에 관해 블로그 글을 정리할지 많은 고민을 했다.
고민 끝에 내린 결론은 CSS이다. 따라서 이번 공부의 주제는 CSS이다. 사실 나는 깡 css를 주로 사용을 해보았고(~~굳이 썼다면 부트스트랩이나 material~~) 이번 프로젝트를 진행하면서 `styled-components`를 처음 사용해보았다. 사용하고서 재사용성, 유지보수성 등의 면에서 너무 흡족했고 이에 따라 `Tailwind CSS`라던가 `SCSS` 등 css 사용을 돕기 위한 다른 스타일 라이브러리 및 프레임워크에 관해서도 알고 싶었다.

### CSS란?

CSS는 기본적으로 HTML의 스타일링을 위한 라이브러리이다.
CSS의 사용을 돕기 위한 여러가지 라이브러리나 프레임워크들이 많이 지원된다.
아래의 사진은 만족도와 사용도
![css](https://velog.velcdn.com/images/skyu_dev/post/c4a9817f-a827-460d-8c8d-e7008c0ca3bb/image.png)

### Sass

> Syntactically Awesome Style Sheets

앞글자를 따서 Sass라고 한다.
Sass는 SASS, SCSS 문법이 있다. 기능은 같지만 아래 사진처럼 문법이 약간씩 차이가 있다.
![Sass](https://velog.velcdn.com/images/aideneverywhere/post/4242f0f2-11a6-49ab-b138-92f4d3e629b6/image.png)
SASS는 identation(들여쓰기)과 newline을 사용하고 SCSS는 curly braces(중괄호)와 semicolon을 사용한다.

### Tailwind CSS

> Atomic CSS

여러 css 의 조합으로 하나의 스타일링을 정의하는 방식과 반대로 Tailwind 는 css 들을 모조리 분해하여 사용자가 자유롭게 조합할 수 있도록 만들었다.
최근 인기와 관심도가 높은 기술 중 하나라고 한다.
더 자세히 말하자면, 모두 같은 specificity class로 구성되어 CSS의 관리와 네이밍을 위한 고민이 필요하지 않다고 한다.

### CSS Modules

> CSS-in-CSS

CSS 파일에 선언한 클래스명이 자동 변경되어 중복을 신경쓰지 않아도 되며, 전체 페이지의 CSS를 렌더링 시 한 번에 로드한다.

### Styled-component

> CSS-in-JS

한 마디로 스타일을 가진 컴포넌트를 정의해서 사용할 수 있다. CSS를 JS를 통해 만들어서 return 구문은 semantic한 컴포넌트들로만 구성되어 있어 가독성 좋은 코드를 만들 수 있다. 컴포넌트로 전달하는 값(JS 함수, 값 공유)에 따라 동적으로 스타일 적용 가능하다.
JS 런타임 과정에서 동적인 클래스명을 생성해준다. 정말 좋은 점은 클래스명이나 id명을 랜덤으로 자동 생성해주기 때문에 중복이 발생할 일이 없다. 하지만 CSS 구문 분석 과정이 필요해서 렌더링 속도가 느리다.

#### 소견

**Sass**
보면서 Sass에서 문법은 SASS보다는 SCSS가 편한 것 같다. 파이썬 개발자는 전자가 편할지도 모르지만 나는 주로 개발 언어가 c언어나 js쪽이기 후자가 편하다.

**Tailwind CSS**
Tailwind CSS는 진짜 방법이 달라서 감이 잡히지도 않는다.

**CSS Modules vs Styled Component**
이 둘의 특징이 정말 상반돼서 신기했다. 지금 Styled-component를 사용하고 있으므로 다른 특징을 지닌 Css Modules로도 개발을 진행해보면 둘의 특징을 확실하게 알 수 있을 것 같다.
