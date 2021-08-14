---
layout: post
title: "깃허브 클론해서 Docker로 서버 빌드하기" 
date: 2021-06-19
category: read 
excerpt: ""

---

## 깃허브 클론해서 Docker로 서버 빌드하기

### Docker 계정 만들기

[여기](https://hub.docker.com/signup)에서 Sign Up

### Docker 설치

[여기](https://hub.docker.com/editions/community/docker-ce-desktop-mac)에서 다운로드

# 1. CLI

> GUI 툴은 아래에 있음

### Github Repository clone하기

```bash
git clone <git주소>
```

### Docker Image 빌드하기

![image](https://user-images.githubusercontent.com/28949235/122640898-db27e580-d13c-11eb-87d9-a98a1de756cd.png)

클론한 repo 디렉토리로 이동해서 서버 개발자분이 작성해주신 Command 실행!

![image](https://user-images.githubusercontent.com/28949235/122640892-d2cfaa80-d13c-11eb-8809-23181ccfa2bd.png)

### 서버 실행하기

![image](https://user-images.githubusercontent.com/28949235/122640943-1fb38100-d13d-11eb-9927-a1099536e278.png)

그 후에 MySQL Table을 Migration해야 될지 안 해야 될 지 몰라서 우선 서버부터 실행해봤다.

![image](https://user-images.githubusercontent.com/28949235/122640968-47a2e480-d13d-11eb-8aa5-b8148e307021.png)

에러 뱉어서 하라는 대로 한 후에 다시 서버 실행

![image](https://user-images.githubusercontent.com/28949235/122640979-5ee1d200-d13d-11eb-9c01-fda4f0b0f2c0.png)

된 건지 만 건지.... 안 된 것 같은데...

![image](https://user-images.githubusercontent.com/28949235/122641035-a8cab800-d13d-11eb-8548-b1359e55042b.png)

postman으로 찍어보니까 안 나옴..( ◠‿◠ ) 



## 2. GUI

![image](https://user-images.githubusercontent.com/28949235/122640862-9b60fe00-d13c-11eb-9f4c-66a8988ca86d.png)
![image](https://user-images.githubusercontent.com/28949235/122640868-a4ea6600-d13c-11eb-886e-6a9b3b2de6c8.png)
