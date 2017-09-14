---
layout: post
title: Travis-CI와 Prviate NPM 모듈을 사용한 테스트 자동화
description: Travis-CI에서 테스트 스크립트가 담긴 Private NPM 모듈을 다운받아 테스트를 자동화 해봅시다
date: 2017-09-14 16:45
author: hosy
category: [tips]
tags: [npm, travis, ci]
comments: true
share: true
display: true
---

회사에서 프로젝트를 하다보면 Public으로 진행하기에는 노출되어서는 안되는 정보가 포함되는 경우 NPM의 Private 모듈을 배포하고 이를 Travis-CI에서 테스트 진행시 다운로드 받아 사용하도록 설정해 줄 수 있습니다.

아래는 @nurigo/api-test 라는 패키지로 올린 경우  .travis.yml 파일의 설정 예입니다.

```yml
language: node_js

node_js:
  - "6"

before_install:
  - 'echo "//registry.npmjs.org/:_authToken=${NPM_TOKEN}" > ~/.npmrc'

script:
  - npm install @nurigo/api-test
  - cd node_modules/@nurigo/api-test; npm test
```

before_install 섹션에 .npmrc 파일에 인증정보를 쓰는 명령을 추가해주어 @nurigo/api-test 비공개 패키지를 읽어 올 수 있도록 해줍니다.

Travis-CI의 Settings 에서 환경변수로 추가해주어 ${NPM_TOKEN}에 넘겨줄 수 있습니다.

>![환경변수](/images/travis-settings.png)

NPM_TOKEN 값은 자신의 PC 계정에 이미 생성된 .npmrc 파일을 열어보면 알 수 있습니다.

> cat ~/.npmrc
