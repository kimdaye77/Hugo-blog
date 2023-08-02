---
title: "소프트웨어 개발 방법론"
date: 2023-07-31T21:03:25+09:00
draft: false
slug: "software-engineering"
tags: ["소프트웨어공학", "개발방법론"]
showComments: true
---

## 현업에서 소프트웨어 개발

현업에서는 소프트웨어 개발을 진행할 때 프로젝트를 어떻게 만들지 설계를 해야한다. 이 때 쓰이는 것은 개발 방법론, 깃 브랜치 전략 등이 있다.
오늘을 팀과 그라운드 룰, 여러 협업 전략에 대해 이야기를 나누고 규칙을 세워봤다. 그 과정에서 소프트웨어 개발 방법론을 전체 강의에서 들으면서 다시금 소프트웨어 개발 실무 수업을 듣던 예전으로 돌아간 것 같았다.. 이 개념들은 질문에서도 가끔 등장하기 때문에 오랜만에 정리하는 시간을 가져본다.

### 소프트웨어 개발 방법론

크게 **Waterfall** 방법론, **Agile** 방법론이 있다. 이 중 `Agile` 방법론이 유연한 대처가 가능해서 많이 쓰인다. `Agile` 방법론 중에서는 주로 Scrum, Kanban이 많이 쓰인다.

### Agile Scrum

일주일~이주일 스프린트에 적합한 프로젝트에 유용하다. 일반적으로 Scrum 팀은 규모가 작다.
<br><br>역할은 다음과 같이 나뉜다.

```
1. 제품 소유주
: 프로젝트의 핵심 이해 당사자로 프로젝트 비전 정의
: 백로그 관리, 종속성 배치, 니즈 우선순위화, 클라이언트 니즈 예측
: 클라이언트와 팀 연락 담당

2. 스크럼 마스터/프로젝트 매니저
: 팀에 작업을 지시해 회의 구성
: 진행률 감시 및 작업 완료 방해 요인 제거의 책임

3. 팀
: 공식적인 리더가 없음. 팀은 전문 지식과 자원을 사용해 목표를 달성
```

**장점**

- 모든 사람이 책임을 공유
- 신속한 개발

**단점**

- 기술과 경험을 갖춘 혁신적인 팀 필요
- 빡빡한 타임라인
- 매일 회의 일정 잡는 것이 어려움
- 다양한 프로젝트에 참여하는 팀 구성원의 우선순위 변화로 인해 결과물 지연 가능성

#### Sprint BurnDown

<!-- prettier-ignore-start -->
{{< chart >}}
type: 'line',
data: {
  labels: ['1', '2', '3', '4', '5', '6', '7'],
  datasets: [{
    label: 'Remaining Effort',
    data: [65, 59, 58, 57, 56, 55, 50],
    tension: 0.2
  }]
}
{{< /chart >}}
<!-- prettier-ignore-end -->

스프린트 작업에 할당된 남은 작업을 기반으로 번다운 추적을 지원한다.(jira 같은 협업툴에서 지원) 따라서 번다운 차트를 보면서 본인의 작업 속도를 객관적으로 파악 가능하다.

### Agile Kanban

Agile Scrum은 짧은 타임라인에 초점을 맞추지만 Agile Kanban 방법론에는 고정 길이 스프린트가 없다. → 작업이 지속되고 제품 제공도 지속된다는 의미이다.<br>작업에 대한 타임라인을 표시하지 않고 항목과 결과물이 완성된 시점을 보여준다. 설정된 역할이 없고 팀 전체가 보드를 소유한다.

**장점**

- 진행중인 작업 양을 제한해서 중요한 작업에 초점을 맞추는데 도움
- 수용 작업량이 빌 때마다 새로운 항목 추가 가능
- 매일 회의 불필요
- 지속적인 개선 가능
- 워크플로우 시각화

**단점**

- 타임라인이 없어 느슨한 작업이 될 수 있음
- 책임자가 없음