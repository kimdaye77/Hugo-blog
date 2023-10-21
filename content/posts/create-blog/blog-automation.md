---
title: "Hugo 블로그 제작기 - 자동화"
date: 2023-08-12T18:27:13+09:00
draft: false
categories: "블로그 제작"
slug: "create-blog-automation"
tags: ["블로그 제작기"]
---

## Hugo 블로그 자동화

오늘은 블로그 자동 배포를 해야지 해야지... 생각만 하다가 드디어 실행에 옮겼다. 자동화를 하려고 검색을 하니까 크게 두 가지 방법이 있는 것 같았다. 1. Github Action을 이용한 방법과 2. Shell Script를 이용한 방법이 있었다. 주로 사람들이 전자를 쓰고 레퍼런스가 많은 것 같아 나도 전자를 선택했다.

### Github Action?

우선, 깃허브 액션에 대해 알아보자. 깃허브 액션은 Github에서 제공하는 CI/CD 솔루션이다. 유저는 uses 라는 명령어로 원하는 작업을 불러올 수 있다.
따라서 이번 포스팅에서는 Github Action 통해서 기존에 로컬에서 hugo 파일을 빌드해서 올리던 것을 Github Action을 통해 빌드하고 배포하도록 자동화했다. 이를 통해 블로그 포스팅용 레포지토리에 push만 하면 포스팅이 자동으로 배포된다!!

### gh-pages

npm의 gh-pages를 이용하면 깃허브를 통해 프로젝트가 변경될 때마다 자동으로 배포해준다.

```sh
# gh-pages 패키지 설치

npm install -D gh-pages
```

해당 명령어를 블로그 프로젝트에서 실행해준다.
~~바보같이 이 명령어를 실행 안해줘서 자꾸 존재하지 않는다고 에러가 났다. ㅋㅋ.. 해당 브랜치명으로 생성도 해보고 하다가 다른 글보고 깨달았다.~~ OTL

```
# package.json
{
  "devDependencies": {
    "gh-pages": "^6.0.0"
  },
  "scripts": {
    "build": "webpack --mode production",
    "predeploy": "npm run build",
    "deploy": "gh-pages -d dist"
  }
}
```

해당 명령어를 실행하면 프로젝트 아래에 `package.json` 파일이 생성되는데 위와 같이 수정하면 된다.

### Github Action으로 hugo build 자동화

과정은 다음과 같다.

```
1. 배포할 브랜치 설정
2. git checkout 을 통해 submodule, git contents를 pull
3. Github secrets 에 config.toml 파일을 저장 후 불러오기
4. hugo --minify 로 ./public 폴더에 내용 생성
5. google adsense를 위한 파일 삽입
6. github page용 repo에 파일 push
```

우선 블로그 포스팅이 올라가는 레포지토리에 `.github/workflows/main.yml`를 하나 생성한다. yml 확장자로만 되어 있으면 파일 이름은 상관없지만 경로는 매우\*100 중요하다.

#### 최종 yml 파일

```yml
name: github pages

on:
  push:
    branches:
      - main # 1번

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2 # 2번
        with:
          submodules: recursive

      - name: copy config.toml # 3번
        run: echo "${CONFIG_TOML}" > config.toml
        env:
          CONFIG_TOML: ${{ secrets.config }}

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "latest"
          # extended: true

      - name: Build
        run: hugo --minify

      - name: site-verification # 5번
        run: echo "${{ secrets.SITE_VERIFICATION }}" > ./public/google34bf590cfe76298c.html

      - name: Deploy # 6번
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }}
          external_repository: kimdaye77/kimdaye77.github.io
          publish_branch: main
          publish_dir: ./public
          user_name: kimdaye77
          user_email: kimdaye77@naver.com
```

#### 키 생성

6번에서도 참 많은 우여곡절을 겪었다. `deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }}`는 원하는 키 이름으로 해당 레포지토리 settings 아래 secrets에 있는 Actions에서 배포 키를 저장하면 된다. secrets 같은 경우는 비밀 키로 해야한다.
![키](img/blog-automation1.png)

```sh
# 키 생성
ssh-keygen -t rsa -C "이메일 주소"

# 공개 키
cat ~/.ssh/id_rsa.pub

# 비밀 키
cat ~/.ssh/id_rsa
```

#### 결과

![실패](img/blog-automation3.png)
실패하면 위와 같이 어떤 부분에서 실패한 지 알려준다.
![성공](img/blog-automation2.png)
10트 넘게 한 끝에 성공!!

### 회고

검색할 때 키워드를 Hugo, 자동화, Github Action 등으로 했는데 레퍼런스가 적었다. 나중에 보니 Hugo 키워드로 인해 결과가 적었던 것이었다...!! 만약 자동화 할 일이 있거나 깃허브 액션을 쓸 경우는 Hugo 키워드는 제외하길.. 검색 결과가 많이 나와서 참고하기 좋다.

#### 이후 할 것

알아본 바로는 github에서 token을 통한 push만 허용되는 변경 등으로 인해 더이상 실행되고 있지 않은 듯?
2번의 과정에서 이 경우는 deprecated라고 떠서 수정해야 할 듯 싶다.
