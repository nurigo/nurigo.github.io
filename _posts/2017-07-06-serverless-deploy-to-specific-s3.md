---
layout: post
title: 서버리스 deploy를 특정 버킷에 배포하기 
description: "서버리스를 이용한 deploy 시, 임의 bucket 을 생성하여 deploy를 매번 하게 되면, 나중에 bucket 수에 제한에 문제가 됩니다."
date: 2017-07-06 14:45
author: billy_kang
category: [articles, serverless, deploy]
tags: [serverless]
comments: true
share: true
---

해당 페이지에서는 Serverless 배포시, 특정 S3 로 배포하는 방법에 대하여 설명하고 있습니다. 

## 짧게 요약
#### 1. S3 에 deploy들를 담을 버킷 생성
#### 2. project의 serverless.yml 에 deploymentBucket 옵션을 추가
#### 3. sls deploy
#### 4. S3 배포확인 
<br>
<br>

## 길게 설명

#### 1. S3 에 deploy들을 담을 버킷 생성합니다. 
>
>테스트로 사용할 버킷을 생성합니다. 저는 "coolsms.billy.projects" 이라는 버킷을 만들었습니다. 
>
>![S3버킷생성](/images/serverless_deploy/create_bucket.png "버킷생성")


#### 2. Project 의 serverless.yml 에 deploymentBucket 옵션을 추가
>
>로컬에서 coolsms.billy.projects 라는 폴더에 serverless 예제로 사용할 Project 를 4개를 
```
sls create -t aws-nodejs -n testProject1 -p ./testProject1
sls create -t aws-nodejs -n testProject2 -p ./testProject2
sls create -t aws-nodejs -n testProject3 -p ./testProject3
sls create -t aws-nodejs -n testProject4 -p ./testProject4
```
>을 사용하여 생성합니다.                                                  
>
>![serverless작업폴더생성](/images/serverless_deploy/create_projects.png "serverless작업폴더생성")
>
>생성된 폴더 안에는 serverless 기본 템플릿 파일들이 구성되어있습니다. 
>![serverless작업폴더생성](/images/serverless_deploy/ls_projects.png "serverless작업폴더생성")
>
>각각의 폴더안에 있는 serverless.yml 을 열어서 아래와 같이 deploymentBucket 을 추가합니다. 
```
service: testProject1
provider:
  name: aws
  region: ap-northeast-2
  runtime: nodejs4.3
  deploymentBucket: coolsms.billy.projects
functions:
  hello:
    handler: handler.hello
```

#### 3. sls deploy
>
>각각의 test 프로젝트를 sls deploy 해줍니다. 
![slsdeploy](/images/serverless_deploy/sls_deploy.png "sls_deploy")



#### 4. 배포확인.
>
>S3 의 "coolsms.billy.projects" 버킷으로 가서 배포한 serverless projects 들이 잘 들어가있는지 확인합니다. 
>
>![s3_deploy](/images/serverless_deploy/s3_deploy.png "s3_deploy")
>
>상단의 Amazon S3 > coolsms.billy.projects > serverless  폴더에 testProject1 ~ testProject4 까지 잘 배포되었네요. 