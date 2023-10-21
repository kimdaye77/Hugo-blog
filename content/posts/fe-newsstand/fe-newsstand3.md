---
title: "[현대 소프티어] FE 뉴스스탠드 #3"
date: 2023-07-19T18:09:15+09:00
draft: false
categories: "현대 소프티어"
slug: "fe-newsstand3"
tags: ["프론트엔드", "newsstand", "현대소프티어"]
---

## FE 뉴스스탠드 3주차

참고 {{< icon "github" >}}
<br>
[fe-newsstand #3](https://github.com/softeerbootcamp-2nd/fe-newsstand/pull/80)
<br>

- [fe-newsstand 7.19](https://github.com/kimdaye77/fe-newsstand/pull/6)
- [fe-newsstand 7.19](https://github.com/kimdaye77/fe-newsstand/pull/7)
  <br>

### Done

오늘은 기능 구현보다는 주로 리팩토링에 힘을 썼다. 오전 시간(10~12시)에는 store와 view를 느슨하게 하기 위해 고민을 했다.
똑똑한 gpt와 블로그를 참고하여 store class를 만들고 observer 함수에 등록했다.
자세히 설명하겠다. ~~(이해가 아직 다 된 것은 아니라 완벽하게 내 코드가 되진 않았다.)~~

### Refactoring

옵저버 패턴으로 리팩토링을 진행하였다. 원래는 콜백 함수를 사용해서 값이 바뀌면 부모에게 전달해주었는데 코드가 많이 복잡했다.
옵저버는 1대 다 구조에 유리하고 상태가 변경되면 알아서 바꿔준다! (리덕스, PubSub 패턴과 유사)
중앙 집중식이라고 생각해서 `store.js`, `observer.js`와 `getter.js`는 core 폴더에 넣어주었다.

#### Store

```js
# store.js
import { FIRST_PAGE_NUM } from "../constants/constants.js";
import { observable } from "./observer.js";
class Store {
  constructor() {
    this.state = observable({
      mode: "light", //추후 다크모드
      view: "grid", //그리드뷰 또는 리스트뷰
      page: FIRST_PAGE_NUM, //현재 페이지
      tabMode: "all", //all 전체언론사 sub 구독한 언론사
      subscribedPress: ["서울경제", "데일리안", "헤럴드경제"],
    });
  }

  setState(newState) {
    for (const [key, value] of Object.entries(newState)) {
      if (!this.state.hasOwnProperty(key)) continue;
      this.state[key] = value;
    }
  }
}

export const store = new Store();

```

#### Observer

```js
# observer.js
let currentObserver = null;

export const observe = (fn) => {
  currentObserver = fn;
  fn();
  currentObserver = null;
};

export const observable = (obj) => {
  Object.keys(obj).forEach((key) => {
    let _value = obj[key];
    const observers = new Set();

    Object.defineProperty(obj, key, {
      get() {
        if (currentObserver) observers.add(currentObserver);
        return _value;
      },
      set(value) {
        _value = value;
        observers.forEach((fn) => fn());
      },
    });
  });
  return obj;
};

```

observable은 객체의 속성에 get, set을 정의하여 관찰 가능한 상태로 만드는 함수이다. 즉, observable 함수를 사용하면 객체의 속성 값을 조회하고 할당할 때 특별한 동작을 추가하여 상태 변경을 감지할 수 있게 된다.

observable 함수를 사용하여 만든 객체의 속성은 Getter와 Setter가 적용되어, 속성의 값이 변경되거나 조회될 때 등록된 옵저버들이 자동으로 호출된다. 이를 통해 상태 변경을 감지하고, 등록된 옵저버들이 자동으로 업데이트되는 상태 관리를 할 수 있다.

따라서 observable 함수를 사용하여 만든 store 객체는 관찰 가능한 상태를 가지며, 속성의 값이 변경되면 등록된 옵저버들이 자동으로 호출되어 상태 변경을 감지할 수 있다. 이를 통해 상태 조회 로직과 상태 변경 로직을 분리하여 코드를 더 모듈화하고, 상태 변화에 따른 반응을 자동화할 수 있다. (gpt의 설명 참고 ㅎㅁㅎ)

#### Getter

```js
# getter.js

import { store } from "./store.js";
import { observe } from "./observer.js";
export function getMode() {
  return store.state.mode;
}
export function getView() {
  return store.state.view;
}
export function getPage() {
  return store.state.page;
}
export function getTabMode() {
  return store.state.tabMode;
}
export function getSubscribedPress() {
  return store.state.subscribedPress;
}

// 옵저버 등록
observe(getMode);
observe(getView);
observe(getPage);
observe(getTabMode);
```

`getter`는 store에 넣어도 된다. 하지만 store에는 순수한 데이터를 저장하고 어댑터 역할로 뷰에 전달하도록 필터 느낌으로 사용하고 싶다면 getter함수들을 따로 빼도 된다.
이 경우, test와 유지보수에 용이하다는 장점이 있다고 한다.
store은 view에 dependency가 없고 메소드 정도는 있어도 된다. 하지만 순수할수록 좋다고 한다. 또한 옵저버 등록하는 부분들이 숨겨져 있는 것 같다고 크롱님께서 피드백을 주셨다. 명확하게 기능을 보이기 위해 어디에 위치시킬지 고민해봐야겠다.

### 마치며

오늘 코드 피드백 시간에 3개 중 하나가 내 코드가 걸렸다. 덕분에 내 코드를 다시 돌아보고 이해하는 과정을 겪었다. 피드백을 반영할 것들을 메모해봤다.

**[스쿼드세션]**

- 상수에 freeze를 사용하면 read only가 되어서 값을 수정할 수 없다.
- 변하는 state는 대문자 사용하지 않기

**[코드리뷰 피드백]**

- core은 개발자들이 볼 때 전체 페이지를 아우르는 느낌이기 때문에 조심해야 함
- file 이름 상단에 모듈 역할 적기
- `getView`의 경우에는 뷰를 얻어오는 것이 아니라 view mode를 얻어오는 것이기 때문에 `getViewMode`와 같이 변경
- camel case와 \_ 중 네이밍 일관성있게 통일시키기
- swtich case에서 target 부분 명확하게 이름짓기
- 이미지, url들은 다 분리하기
- 주석, console.log도 커밋할 때 지우기
- template literal은 html처럼 들여쓰기 등으로 보기 좋게
  => 만약 너무 길어지면 로직 빼내기
- reducer 사용해보기
- parentelement 같은 경우는 여러 번 중첩해서 쓰지 않기
  => DOM은 변경에 취약하기 때문에 많이 쓰면 파악 어려워짐
