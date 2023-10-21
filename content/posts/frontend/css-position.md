---
title: "[FE] CSS-position 속성"
date: 2023-08-09T20:47:34+09:00
draft: false
categories: "프론트엔드"
slug: "css-position"
tags: ["프론트엔드"]
---

## CSS

오늘은 Cartag 프로젝트에서 `position` 속성 중 `sticky`를 사용해봤다.<br>
`sticky`는 최근에 추가된 속성으로 `fixed`와 `static`의 속성을 정한 값에 따라 적용할 수 있는 좋은 기술이다.<br>
~~요즘에는 참 좋은 기술이 많이 생긴 것 같다.~~

### Position

`position` 속성은 html 태그의 위치를 정할 수 있으며 여러 가지 속성값이 있다.

```css
# 사용법

position: {원하는 속성값}

# 예시

#box1 { position:inherit }
#box2 { position:static }
#box3 { position:absolute }
#box4 { position:relative }
#box5 { position:fixed }
#box6 { position:sticky }
```

속성값으로는 다음과 같이 있다.

#### static

**기본값**이다. 다른 요소와의 관계에 의해 auto로 배치된다.
좌표를 설정할 수 없다.

#### inherit

부모 요소의 태그 속🕊️성값을 상속받는다.

#### absolute

부모 요소를 `relative`로 주고 본인(자식)을 `position: absolute`와 `top, bottom, left, right` 좌표를 지정해주면 절대 좌표로 위치를 지정할 수 있다.
<br>absolute 속성 값은 아래의 relativ와 자주 쓰인다.
겹치는 요소일 때 많이 사용한다.

#### relative

원래 있던 위치를 기준으로 좌표를 지정한다.

#### fixed

스크롤에 관계 없이 좌표를 고정한다. Nav 바 같은 곳이나 Footer에 많이 쓰인다.

```css
# 예시

position: fixed;
top: 0px;
left: 0px;
z-index: 999; //구구구..🕊️
```

#### sticky

static과 fixed가 섞인 느낌! 원래는 static하게 존재하다가 스크롤시 지정 지점에서 요소를 고정시킨다.

```
# sticky 사용 시 주의점

1. sticky 속성을 갖는 요소의 부모 태그에 height이 지정되어 있어야 한다.
   (부모 height 높이 값만큼 고정된다.)

2. 부모요소중에 overflow: hidden, auto, scroll 속성이 적용되어 있으면 안된다.
```

### 적용 영상

나는 `top: {원하는 높이px}`만큼 줬기 때문에 영상에서 보다 싶이 저 픽셀까지는 스크롤 되다가(static 속성) 이후로는 fixed 속성과 같이 고정된 곳에 위치하도록 동작한다. sticky는 영상이나 그림으로 보는 것이 이해가 쉽다.

<video controls>
    <source src="https://github.com/softeerbootcamp-2nd/A2-CarTag/assets/63107805/3aef0705-ce46-4cbd-8891-bd0cc427d75e" type="video/mp4" />
</video>
