---
layout: post
title: "git 다른사람 브랜치 내용 merge 전 받아보기" 
date: 2020-12-05
category: read 
excerpt: ""

---

# git 다른사람 브랜치 내용 merge 전 받아보기

> 클론하지 않고 다른 사람의 repo를 리모트 추가하는 방법

```bash
$ git remote add 추가할리모트이름 https://github.com/다른사람유저네임/레포이름.git
$ git fetch 추가한리모트이름
$ git checkout -t 추가한리모트이름/브랜치이름
```

출처는..갓수진님..s2s2