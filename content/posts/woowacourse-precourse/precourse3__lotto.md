---
title: "[우테코 프리코스] 로또 #3"
date: 2023-11-7T19:24:11+09:00
draft: false
categories: "우테코 프리코스"
slug: "precourse3"
tags: ["우테코", "프리코스", "로또"]
---

## 우테코 프리코스 3주차

[로또 fork repository](https://github.com/kimdaye77/javascript-lotto-6)
<br>
1주차와 동일하게 내 레포지토리로 포크하고 클론해서 작업을 진행한다.

### 구현할 기능 목록

과제 요구 사항에서 기능 구현 전, `docs/README.md`에 구현할 기능 목록을 정리할 것을 명시하고 있다.

```md
# 미션 - 로또
## ✏️ 구현할 기능 목록

### `App.js`의 `play` 메서드를 통해 프로그램 시작

- [x] play 메서드 생성

### 로또 로직

- [x] 로또 구입 금액 입력

  - 구입 금액 입력 안내 문구 출력<br>
    `구입금액을 입력해 주세요.`

  - 잘못된 값 입력 시 **throw문을 통해 예외 발생 및 애플리케이션 종료**<br>
    - 1,000원으로 나누어 떨어지지 않는 경우

- [x] 구입 금액에 해당하는 만큼 로또 발행(로또 1장 당 1,000원)

  - 구매 갯수 문구 출력<br>
    `${로또 구입 금액/1000}개를 구매했습니다.`

  - 각 로또 번호 출력(구매 갯수 만큼)
    - `Random API`를 이용하여 1-45 사이 값 6개 선택
    - 로또 번호는 오름차순으로 정렬하여 보여준다.


- [x] 당첨 번호 입력

  - 당첨 번호 입력 안내 문구 출력<br>
    `당첨 번호를 입력해 주세요.`
 
  - 잘못된 값 입력 시 **throw문을 통해 예외 발생 및 애플리케이션 종료**<br>
    - 쉼표로 구분했을 때, 번호 6개
    - 각 번호가 1-45 범위 내의 수가 아닌 경우

- [x] 보너스 번호 입력

  - 보너스 번호 입력 안내 문구 출력<br>
    `보너스 번호를 입력해 주세요.`
 
  - 잘못된 값 입력 시 **throw문을 통해 예외 발생 및 애플리케이션 종료**<br>
    - 번호가 1-45 범위 내의 수가 아닌 경우


- [x] 로또 결과 출력

  - 당첨 내역 출력
  - 수익률은 소수점 둘째 자리에서 반올림
  - 아래는 예시이다.
  
  ```
    당첨 통계
    ---
    3개 일치 (5,000원) - 1개
    4개 일치 (50,000원) - 0개
    5개 일치 (1,500,000원) - 0개
    5개 일치, 보너스 볼 일치 (30,000,000원) - 0개
    6개 일치 (2,000,000,000원) - 0개
    총 수익률은 62.5%입니다.
  ```
```

## 회고

### 테스트

#### 첫 번째
이번에는 Lotto 클래스 관련한 테스트를 작성해보았다.
원래 계획으로는 로또 클래스 관련해서 1. 오름차순 정렬이 잘 되는지, 2. 사용자 입력 예외 처리가 되는지를 테스트 작성하고 싶었다.
하지만 구현을 먼저 한 후, 테스트를 진행하려고 하니까 알맞는 테스트가 없었다.
 이래서 테스트 주도 개발이라는 방법론(TDD)이 존재하는구나... 싶었다.

💡 **해결 방법**<br>
그래도 목표한 것 중 1번 오름차순 정렬은 정렬 코드가 원래 lottoUtils.js에 있었지만 Lotto 클래스 내부로 위치를 바꾸면 테스트를 작성할 수 있을 것 같아서 수정했다.

```
{{<highlight js
"lineos=table, hl_lines =26-35">}}
# LottoTest.js

import { MissionUtils } from "@woowacourse/mission-utils";
import Lotto from "../src/Lotto.js";

const getLogSpy = () => {
  const logSpy = jest.spyOn(MissionUtils.Console, "print");
  logSpy.mockClear();
  return logSpy;
};

describe("로또 클래스 테스트", () => {

  test("로또 번호의 개수가 6개가 넘어가면 예외가 발생한다.", () => {
    expect(() => {
      new Lotto([1, 2, 3, 4, 5, 6, 7]);
    }).toThrow("[ERROR]");
  });

  test("로또 번호에 중복된 숫자가 있으면 예외가 발생한다.", () => {
    expect(() => {
      new Lotto([1, 2, 3, 4, 5, 5]);
    }).toThrow("[ERROR]");
  });
  
  test("로또 번호는 오름차순으로 정렬되어 출력해야 한다.", () => {
    const logSpy = getLogSpy();

    new Lotto([5, 3, 1, 4, 2, 6]);

    const log = "[1, 2, 3, 4, 5, 6]";

    expect(logSpy).toHaveBeenCalledWith(expect.stringContaining(log));
  });
});
{{</highlight>}}
```

#### 두 번째
두 번째로는 자꾸 맞게 테스트를 실패해서 원인을 도통 몰랐다.
![테스트 실패](img/precourse3-1.png)

🔨 **시도 1**<br>
문자열이 포함 안 됐다고 메시지가 출력되었기 때문에 원인을 알았다. 원인은 배열을 그대로 `Console.print(this.#numbers 배열)`을 출력해서 문자열로 인식을 하지 못하는 것 같았다. 그래서 첫 번째로는 문자열로 바꿔주는 `JSON.stringfy`를 이용해서 출력을 하려고 했다. 하지만 문제가 있었다. 이 방법은 `,(콤마)` 뒤 공백이 존재하지 않기 때문에 테스트 통과가 되지 않았다.

💡 **해결 방법**<br>
두 번째 시도 방법으로는 `Console.print(`[${this.#numbers.join(', ')}]`);` `join`과 템플릿 리터럴을 사용해서 해당 문자열 출력 포맷으로 바꾼 다음, 출력해주었다.

### 요모조모
#### #prefix
자바스크립트에서 자바의 접근 제어 지시자(private, public)와 비슷한 기능을 하는 # prefix에 대해 알게 되었다. 아주 유용하다.
#### 요구 사항 잘 읽기
- 에러메시지 출력 후 입력 다시하는 것을 몰랐다…계속 삽질 👻
#### 비동기 처리 꼼꼼하게
`this.winningResult = await getWinningNumbersAndBonus();` 여기서
await을 안 적고 리턴해서..계속 값을 못 읽는 이슈가 있었다.

✔️ **최종 결과** 💯<br>
![결과](img/precourse3-2.png)
결과는 모두 통과!!

👇🏻 아래는 PR 링크이다. 👇🏻<br>
[PR](https://github.com/woowacourse-precourse/javascript-lotto-6/pull/100)

![예제 결과](img/precourse3-3.png)
pr링크를 제출하면 위와 같이 예제 테스트가 모두 통과하는 것을 확인할 수 있다.