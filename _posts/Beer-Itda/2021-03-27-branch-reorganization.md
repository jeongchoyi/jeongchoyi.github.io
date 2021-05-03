---
layout: post
title: "브랜치 정리하기" 
date: 2021-03-27
category: read 
excerpt: ""

---

# 브랜치 정리하기

release 브랜치 관련해서 실수한 것들 정리하기,,^.ㅠ  
[여기](https://iamcho2.github.io/2021/03/22/branch-rule-git-flow)에도 써놨지만 내가 release 브랜치를 잘못 이해하고 있었다.

### release.....

feature를 release에서 파는 게 아니었다...!!!  
해당 release 버전의 feature를 관리하는 브랜치인 줄 알았는데,  
feature는 develop에서 판다.

1. develop 브랜치에서 배포할 수 있는 수준의 기능이 모이면 (또는 정해진 배포 일정이 되면)  
   release 브랜치를 분기한다.

* release 브랜치를 만드는 순간부터 배포 사이클이 시작
* release 브랜치에서는 배포를 위한 최종적인 버그 수정, 문서 추가 등 release와  
  직접적으로 관련된 작업을 수행한다.
* 직접적으로 관련된 작업 외에는 release 브랜치에 새로운 기능을 추가로 머지하지 않는다.



2. release 브랜치에서 배포 가능한 상태가 되면

* main 브랜치에 머지한다 (이때, merge한 커밋에 release 버전 태그를 부여한다)
* 배포를 준비하는 동안 release 브랜치가 변경되었을 수 있으므로  
  배포 완료 후 develop 브랜치에도 병합한다.

### 수습하기

<img src="https://user-images.githubusercontent.com/28949235/112749751-6d699300-8fff-11eb-8f4a-f5f549f61368.png" alt="image" style="zoom:50%;" />

작업하던 거 stash로 빼놓고  
`develop`에서 `release-0.0.1`을 pull 땡긴다. push도 해준다. (release 브랜치가 앞에 있으므로)  
github에서 default branch도 `develop`으로 바꿔준다.  
그 후 `release-0.0.1` 브랜치를 삭제한다.

그 후 `develop`에서  `feature/11` 브랜치를 파고 stash 다시 풀기!

### feature-14? feature/14?

`feature/14`가 feature 브랜치에서 14라는 브랜치를 파는건 줄 알았는데  
그냥 브랜치 이름을 feature/14로 만들면  
feature라는 폴더가 만들어진다

![image](https://user-images.githubusercontent.com/28949235/112749894-45c6fa80-9000-11eb-8c9f-c3add792a4e2.png)   

근데 난 어차피 여러개의 작업을 동시에 진행할 일이 없기 때문에...  
이번 프로젝트에선 feature-14로 간다!

### 커밋 메세지 closing keyword

커밋 메세지에 안 쓰고 커밋 description에 closing keyword 써도 닫힌다고 한다...

