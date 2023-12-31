---
title: "Hugo 블로그 제작기"
date: 2023-07-15T20:36:30+09:00
draft: false
categories: "블로그 제작"
slug: "create-blog"
tags: ["블로그 제작기"]
---

## Hugo와 Github page를 이용한 블로그 제작

### 블로그 제작 결심

원래도 공부하고 시간이 지나면 까먹고 같은 것을 찾아보는 것을 깨닫고 새롭게 안 것을 기록해야 할 필요성을 느꼈다. 생각만 하고 흐지부지 되던 차에 과 방학 행사 중 하나로 블로그 챌린지를 진행한다고 해서 블로그를 개발하게 되었다. 또한 4학년 여름 방학 2개월동안 현대 소프티어 부트캠프를 수료하는데 여기서 배운 것들을 기록하면 정말 좋을 것 같았다.

### Hugo를 선택한 이유

**jekyll**

```
- ruby 기반
- 한글 레퍼런스가 많다
- 빌드 시 시간이 오래 걸린다. (이게 가장 큰 단점인듯)
```

**<mark>Hugo</mark> {{< icon "check" >}}**

```
- Golang 기반
- 가장 빠르다.
- 한글 레퍼런스가 적다.
```

**Hexo**

```
- JavaScript 기반
- 속도는 jekyll보다 빠르고 hugo보다 느리다.
- 중국 레퍼런스가 많다.
```

이렇게 세 가지가 유명한 것 같다. 검색해보니까 jekyll에서 hugo로 넘어간 글이 보이고 hugo가 빌드 시 빠르다는 것, 그리고 테마를 구경하고 마음에 든 것이 있어서 hugo를 선택했다.

### Hugo 설치

설치는 os에 따라 다르다.
현재 2개월 간 시한부 맥북 신세라 (...) brew install을 통해 설치해주었다.
윈도우는 본인이 사용하고 싶은 테마가 SCSS를 포함하고 있으면 확장 버전을 따로 설치해야 하는 것 같았다.

```bash
# hugo 설치
$ brew install hugo

# hugo 버전 확인
$ hugo version
```

### Git 레포지토리 생성

Git 레포지토리는 총 2개 생성해주면 된다.

1. 블로그 포스트 저장용
2. 렌더링용 페이지 (github.io)
   2번의 경우에는 원하는 레포지토리 이름이 있더라도 `{username}/github.io`로 하고 배포한 다음, 이름을 수정해야 한다.

### Hugo 블로그 프로젝트 생성

원하는 프로젝트 이름으로 hugo 프로젝트 `hugo new site blog`를 생성한다. 나의 경우에는 blog로 프로젝트 이름을 지정했다.

### Hugo 테마 적용

테마마다 다운로드하는 방법이 친절하게 설명되어 있다. 나의 경우에는 [congo](https://github.com/jpanther/congo) 테마를 사용했다. 여기서부터는 Go 언어도 설치가 된 상태여야 한다. Go 역시 brew를 통해 설치했다.

## Hugo 프로젝트 디렉토리 구성

테마마다 디렉토리 구조가 다른데 형식(이름)을 같게 해주어야 한다. 아래는 내가 적용한 테마의 기본 구조이다.

```
.
├── assets
│   └── img
│       └── author.jpg
├── config
│   └── _default
├── content
│   ├── _index.md
│   ├── about.md
│   └── posts
│       ├── _index.md
│       ├── first-post.md
│       └── another-post
│           ├── aardvark.jpg
│           └── index.md
└── themes
    └── congo
```

이후로는 샘플과 깃허브 코드들을 보면서 `config/_default/params.toml`등을 원하는 설정으로 변경했다.

## Github 연동

### Repository 연결

#### 1. 포스트 관리용으로 만든 레포지토리와 연결

`git remote add origin <repo 주소>`

#### 2. 호스팅용 레포지토리와 연결

`git submodule add -b main <repo 주소> public`

### 포스트 업로드

이후 포스트를 쓰고 `hugo -t <테마이름>`을 하면 public에 html 파일이 생성된다. 생성된 public 디렉토리에 들어가서 변경된 부분들을 commit 해주면 본인의 github page 주소에 반영이 된다.

## Hugo와 Github page를 이용한 블로그 제작 총평

### 블로그 제작 후기

> 테마가 예쁘고 다크모드 구현, 태그, 검색 등 다양한 기능이 들어있어서 좋았다.  
> 내가 고른 테마는 다국어 번역 기능을 지원하지만 한국어를 지원해주지 않았다. 나중에 시간적 여유가 생긴다면 한국어로 pr을 날려봐도 좋을 것 같다.

### 향후 계획

1. 자동 배포를 통한 자동화
2. 도메인을 구매 후 적용 ~~(가능하다면)~~
