---
title: "[Project] Cartag 프로젝트 #2"
date: 2023-08-02T21:28:50+09:00
draft: false
slug: "프로젝트"
tags: ["타입스크립트", "리액트", "프로젝트"]
---

## Cartag 프로젝트 #2

오늘은 제너럴 컴포넌트를 구현했다. 개발하면서 새로운 것들을 많이 알게 됐는데 그 중 두 가지를 소개하겠다.

### Input 태그 타입

`input`은 항상 `form` 제출 입력용으로 `text`만 사용했는데 이번에 **슬라이드 마커**를 구현하게 되면서 새로 알게 된 쓰임이 있다.<br>
바로 `type`을 `range`로 설정하면 슬라이드 바를 조정하여 범위 내의 숫자를 선택할 수 있는 입력 필드가 생성된다!
이외의 `type`의 종류는 무엇이 있을까 궁금해서 더 찾아보았다.<br>

#### type의 종류

```
Text // 텍스트 입력 필드
Radio // 라디오 박스 (단일 선택)
CheckBox // 체크 박스 (다중 선택)
Submit // 폼 양식 제출
Button // 버튼
Reset // 폼 초기화
File // 파일 선택
Date // 날짜 정보 입력
Number // 숫자 정보 입력
Email // 이메일 형식 입력
Color // 색상 선택
Range // 슬라이드 바
Search // 검색 바
Select-box // 옵션 선택 가능
```

위와 같이 많은 기능을 지원해준다.

### useId

`useId`는 리액트 훅 중 하나이다. 이름에서 알다싶이 고유한 `id`를 자동으로 생성해주는 기능을 한다.
사용법은 기존의 훅 사용법과 동일하다.

#### 사용법

```ts
# 사용 예시
import React, { useId } from "react";

const id = useId();
```

`useId` 훅을 `import` 해주고 변수에 저장하고 나중에 사용하면 된다. 간단!<br>
하지만 주의점이 있다.

#### 주의할 점

- CSS selector 사용 X
- 변경해서는 안 되는 값에는 사용 X
