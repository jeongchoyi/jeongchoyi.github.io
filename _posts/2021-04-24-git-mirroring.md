---
layout: post
title: "github repo를 커밋로그 포함해서 옮기기" 
date: 2021-04-24
category: read 
excerpt: ""

---

# github repo를 커밋로그 포함해서 옮기기

기존 개인 repo를 organization으로 옮길 일이 생겼다.  
[여기](https://oizys18.github.io/2020-08-03/Git-mirroring) 게시글을 그대로 따라하다가

```
 ! [remote rejected] refs/pull/1/head -> refs/pull/1/head (deny updating a hidden ref)
 ! [remote rejected] refs/pull/1/merge -> refs/pull/1/merge (deny updating a hidden ref)
```

이런 오류가 났다. [스택오버플로우](https://stackoverflow.com/questions/34265266/remote-rejected-errors-after-mirroring-a-git-repository) 에서

`git clone --mirror` 말고 `git clone --bare` 사용하라고 해서 따라했더니 해결됐다.

### instruction

1. 터미널 열기
2. `$ git clone --bare https://github.com/원래repo.git`
3. `$ cd 원래repo`
4. `$ git push --mirror https://github.com/목적지repo.git`

끝 ( ◠‿◠ ) 

[여기](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/duplicating-a-repository) 참고했음 !

