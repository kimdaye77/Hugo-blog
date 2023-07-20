---
title: "[현대 소프티어] FE 뉴스스탠드 #4"
date: 2023-07-20T18:12:04+09:00
draft: false
slug: "fe-newsstand4"
tags: ["프론트엔드", "newsstand", "현대소프티어"]
---

## FE 뉴스스탠드 3주차

참고 {{< icon "github" >}}
<br>
[fe-newsstand #3](https://github.com/softeerbootcamp-2nd/fe-newsstand/pull/80)
<br>
[fe-newsstand 7.20](https://github.com/kimdaye77/fe-newsstand/pull/8)

### Done

어제 적은 피드백 일부를 반영했다. 오늘은 오전 시간(10~12시)을 제외하고 2시~5시까지 회사 설명회를 듣느라 개발에 시간을 많이 쏟지 못했다.

**[코드리뷰 피드백]**
<br>
반영한 것은 ~~취소선~~ 표시

- ~~core은 개발자들이 볼 때 전체 페이지를 아우르는 느낌이기 때문에 조심해야 함~~
- file 이름 상단에 모듈 역할 적기
- `getView`의 경우에는 뷰를 얻어오는 것이 아니라 view mode를 얻어오는 것이기 때문에 `getViewMode`와 같이 변경
- camel case와 \_ 중 네이밍 일관성있게 통일시키기
- swtich case에서 target 부분 명확하게 이름짓기
- ~~이미지, url들은 다 분리하기~~
- 주석, console.log도 커밋할 때 지우기
- template literal은 html처럼 들여쓰기 등으로 보기 좋게
  => 만약 너무 길어지면 로직 빼내기
- reducer 사용해보기
- parentelement 같은 경우는 여러 번 중첩해서 쓰지 않기
  => DOM은 변경에 취약하기 때문에 많이 쓰면 파악 어려워짐

=> 이렇게 보니까 리팩토링을 많이 한다고 했는데 적게 했다. 하면서 고민이 생겼는데 이미지 url, 데이터 경로들을 constants.js로 분리하는 과정에 생긴 고민이다. 자세한 것은 [아래(고민점)]({{< ref "#고민점" >}})에 후술하겠다.

#### 경로 분리 이유

데이터나 이미지 같은 경우는 로컬이 아니라 서버에 저장되는데 분리하지 않을 경우 유지 보수가 어렵다.
따라서 개발 시에도 따로 분리해서 적어주는게 이후의 수정 및 개발을 위해서도 좋다고 한다.

#### Object.freeze()

`Object.freeze()` 메서드는 객체를 변경할 수 없게 만든다.(**read only**)
<br>
아래의 경우와 같이 값을 바꾸려고 하면 오류를 던지는 것 같다.

```js
const obj = {
  prop: 42,
};

Object.freeze(obj);

obj.prop = 33;
// Throws an error in strict mode

console.log(obj.prop);
// Expected output: 42
```

#### Object.seal()

`Object.freeze()`를 공부하다가 `Object.seal()`이란 것도 존재하는 것을 알았다.

```js
// 1. 프로퍼티를 변경하는 경우
obj.a = 200;
console.log(obj.a);
// 200

// 2. 프로퍼티를 제거하는 경우
delete obj.a;
console.log(obj.a);
// 200

// 3. 새로운 프로퍼티를 추가하는 경우
obj.b = 500;
console.log(obj.b);
// undefined
```

무슨 차이인지 알겠는가?
<br>
결론적으로는 `freeze`가 제한이 더 많다. `seal`은 프로퍼티를 수정할 수 있지만 `freeze`는 불가능하다.
공통점으로는 전달된 객체들은 확장할 수 없게 되어 새로운 속성을 추가할 수 없다.
전달된 객체 내부의 모든 요소들은 구성 불가능하게 되어 삭제할 수 없다.

### After Refactoring

```
{{<highlight js
"lineos=table, hl_lines = 3-5">}}
# constants.js

(생략)
export const ICON_IMG_PATH = "../assets/icons/";
export const DATA_PATH = "../data/";
Object.freeze({
DATE_UPDATE_TIME,
RECENT_NEWS_CNT,
ROLLING_WAIT_TIME,
ROLLING_DIFF_TIME,
FIRST_PAGE_NUM,
LAST_PAGE_NUM,
PRESS_CNT,
SNACKBAR_WAIT_TIME,
PRESS_LOGO_IMG_PATH,
ICON_IMG_PATH,
});
{{</highlight>}}

```

### 마치며

#### 고민점

html의 이미지 태그의 경로는 어떻게 바꿀지 아직 고민중이다. app.js로 진입점을 하나로 설정해서 모듈들을 import하려고 했는데 html에서 constants들을 쓰자니 script로 불러와야 한다... 어떡해야 할지 몰라서 아직 변경은 못했다.

#### 앞으로 할 일

집에 가서 이번주 한 일의 데모 영상을 촬영해야 한다. 길이는 1~5분 내외로 삽질한 것 내용을 위주로 설명해야 한다.
나는 아마 처음에 구독한 언론사를 콜백으로 변수를 관리하다가 이후 파라미터 관리가 복잡해져서 store와 옵저버 패턴으로 리팩토링한 과정을 설명할 것 같다.
