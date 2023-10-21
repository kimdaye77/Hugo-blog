---
title: "[현대 소프티어] FE 뉴스스탠드 #8"
date: 2023-07-25T15:04:30+09:00
draft: false
categories: "현대 소프티어"
slug: "fe-newsstand8"
tags: ["프론트엔드", "newsstand", "현대소프티어"]
---

## FE 뉴스스탠드 4주차

참고 {{< icon "github" >}}
<br>
[fe-newsstand #4](https://github.com/softeerbootcamp-2nd/fe-newsstand/pull/104)
<br>
[fe-newsstand 7.25 구독하기 기능 관련 상세 사항 구현](https://github.com/kimdaye77/fe-newsstand/pull/14)
<br>

### 구현한 기능 (7/25)

- [x] 글자수 많은 언론사 alert 버그 문제 해결
- [x] 이미지 호버시 확대 5% => 리스트뷰 구독한 언론사에도 적용
- [x] 선택한 카테고리 외 마우스 호버 시 밑줄
- [x] 타이포그래피 스타일 파일 분리 및 적용
- [x] 구독한 언론사 탭 카테고리 스크롤도 가능하도록 구현
- [x] 팝업창 한 번 만들고 display로 컨트롤
- [x] [리팩토링] 함수 모듈화 => drawCategory, mainview

### 학습 내용

#### prototype

`"abc".charAt === "dfg".charAt`을 하면 결과값은 `true`로 나온다. prototype에 있기 때문에 메소드는 동일한 메소드를 가리킨다.
더 자세히 알아보자.

> 자바 스크립트는 모든 것이 객체다.

라는 말이 있다. 아래의 예시를 보면

```js
class Foo {
  constructor(name) {
    this.name = name;
  }
  getName() {
    return this.name;
  }
}
```

을 하고 마찬가지로 `new Foo("crong").getName === new Foo("newcrong").getName`을 하면 `true`가 결과로 나온다. 하지만

```js
class Foo {
  constructor(name) {
    this.name = name;
    this.getName = () => {
      return this.name;
    };
  }
}
```

위와 같이 `constructor` 안에 `getName` 메소드를 넣으면 결과는 `false`이다. 이유는 바로 자바 스크립트의 `prototype`과 관련이 있다.
js에는 상속의 개념이 없다. 비슷한 개념으로 `native api`(prototype)이 있다. 이는 객체가 생성되면 상위 메소드를 **share**하는 개념이다. 최상위 object까지 prototype chain 연결을 통해 `toString`까지 최후에는 다다른다.
extends를 통해 사이에 새로운 prototype을 만들 수도 있다.
<br><br>
cf) 문자열은 객체가 아닌 `primitive type`이지만 `"string".method`이렇게 `.`을 붙이는 순간 객체가 된다. ~~`Wrapper Object`~~

#### 이벤트 위임

이벤트 위임은 이벤트 버블링(전파)을 방지해준다.

- `event.target`은 밖을 눌러도 `leaf node`를 가리키기 때문에 이벤트 위임이 가능하다.
- 위임으로 구현하면 메모리의 이벤트 핸들러 갯수를 감소해서 메모리 효율성을 증가할 수 있다.

[stopPropagation()](https://developer.mozilla.org/ko/docs/Web/API/Event/stopPropagation)이라고 이벤트 위임 외에도 이벤트 버블링을 방지하는 메소드도 존재한다.

**[번외]**
리액트는 이벤트위임을 직접해준다.
(onClick 이벤트를 등록하면 이벤트 리스너들은 document로 올라가서 모이고. 클릭이벤트가 실행되면 분기별로 등록한 이벤트를 처리해준다.)

#### state 관리

- 싱글페이징 구조는 클라이언트가 상태를 가지면 안 되기 때문에 상태 관리 별도로 필요

  1. Flux 아키텍쳐
  2. MVC도 가능(옵저버패턴)
  3. Mediator 패턴(state를 둬서 전달자처럼 상태가 바뀐 것을 알려준다.)

<br> 그 외 여러가지가 존재한다.

- store는 관련있는 애들끼리 모아놓기! 아니면 관련 없는 함수가 쓸데없이 실행된다.
- react는 diff 알고리즘이 있어서 렌더링 변경시 이전 상태와 현재 상태를 비교해서 달라질 경우에만 DOM 부분을 렌더링한다.

### 마치며

- 이제 리스트뷰 새로고침 시 랜덤 구현과 드래그 할 때 뒷부분이 가려지는 문제를 해결하면 얼추 기능 완성에 가까워진 것 같다.
- 마지막에 버그를 발견했는데 못 고치고 집을 가서 슬프다.. 내일 아침에 바로 해야겠다.
  To do. 25개 이상일 때 삭제하면 가끔 여러개 삭제됨.
