---
layout: post
title: 트래비스에서 npm prviate modules 사용하기
description: "description"
date: 2017-08-09 16:45
author: Wiley
category: [tips]
tags: [npm, travis, ci]
comments: true
share: true
display: true
---

테스트 스크립트가 @nurigo/api-test 라는 Private NPM module 에 올려져 있는 경우  .travis.yml 파일을 아래와 같이 설정해 줍니다.

before_install 섹션에 .npmrc 파일을 만들어주도록 합니다. ${NPM_TOKEN}은 Travis-CI의 Settings 에서 환경변수로 추가해 줍니다.


>![환경변수](/images/travis-settings.png)

NPM_TOKEN 값은 자신의 PC 계정에 이미 생성된 .npmrc 파일을 열어보면 알 수 있습니다.

> cat ~/.npmrc


```
language: node_js

node_js:
  - "6"

before_install:
  - 'echo "//registry.npmjs.org/:_authToken=${NPM_TOKEN}" > ~/.npmrc'

script:
  - npm install @nurigo/api-test
  - cd node_modules/@nurigo/api-test; npm test
```
