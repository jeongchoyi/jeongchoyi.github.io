---
layout: post
title: "git flow 브랜치 규칙" 
date: 2021-03-22
category: read 
excerpt: ""

---

# git flow 브랜치 규칙

> [**A successful Git branching model**](https://nvie.com/posts/a-successful-git-branching-model/)

![image](https://user-images.githubusercontent.com/28949235/111961598-96d47b80-8b34-11eb-8237-384afdb4873f.png)

### 브랜치 종류와 설명

- master : 제품으로 출시될 수 있는 브랜치, 배포 Release(Prod) 버전의 소스가 들어있는 branch
  - 기본적으로 github 저장소를 생성하면 있는 branch이다. 배포이력을 관리하기 위한 용도로 사용한다. 
- develop : 다음 출시 버전을 개발하는 브랜치, 개발버전의 소스가 들어있는 branch
  - 일반적으로 Master branch에 병합하기 전 최종 개발버전의 소스가 들어있다. 다음 Release될 버전의 소스라고 생각하면 된다.
- feature : 기능을 개발하는 브랜치
  - 개발자들이 기능개발을 위하여 생성/이용 하는 branch이다. 개발이 완료되면 develop와 병합하여 다른 사람들과 공유한다.
- release : 이번 출시 버전을 준비하는 브랜치
- hotfix : 출시 버전에서 발생한 버그를 수정 하는 브랜치, Master branch의 오류사항을 수정하는 branch
  - Feature > Develop > Master의 병합순이 아니라 Master에서 급하게 수정해야하는 경우에 사용한다. Master에서 직접 branch를 분기하여 생성하며 수정 후 Develop가 아닌 Master에 병합하여 배포한다.

> 항상 유지되는 메인 브랜치들(master, develop)과 일정 기간 동안만 유지되는 보조 브랜치들(feature, release, hotfix)   
> 출처는 [여기](https://woowabros.github.io/experience/2017/10/30/baemin-mobile-git-branch-strategy.html)와 [여기](https://www.kyungyeon.dev/posts/13)



### rebase, --no-ff

보통 커밋 그래프를 깔끔하게 만들기 위해 rebase를 하거나 --no-ff 옵션을 사용하지만,  
이번 프로젝트는 우선 나 혼자 진행하기 때문에 어차피 깔끔한 커밋 그래프가 만들어 질 것 같다 ..  
( 내가 한 이슈에 여러개의 커밋을 하지 않는 이상은 아래와 같은 그래프가 만들어지겠지!)

![image](https://user-images.githubusercontent.com/28949235/111963257-a3f26a00-8b36-11eb-8af2-ee8ddc1b2f57.png)

그래서 사용하지 않을 예정..~

### 이슈 넘버? 기능 설명? 버전 넘버?

- **Master**
- **Develop**
- **Feature**/(Issue_number) or (Feature_name) / (Short Description)
- **Release**/(version_number)
- **Hotfix**/(Issue_number) or **Issue**/(Issue_number)

보통은 이렇게 이슈 넘버나 버전 넘버를 다는데...    
이번 프로젝트에서는 최대한 한 이슈에 대해서는 한 개의 커밋을 하는 것을 목표로 할 것이기 때문에  
feature branch 같은 경우엔 어차피 `feature/<issue_number>`가 될 것이..고 (제발)

근데 문득 1인개발인데 feature에서 또 issue number로 브랜치를 팔 필요가 있나? 싶어서  
그냥 feature-(issue_number)로 팔 것.. ◠‿◠ 그냥 원래 이게 맞는 것 같기도 하고 

release branch 같은 경우에도 release 브랜치를 항시 유지하는 것이 아니라  
release-1.0.0 같은 식으로 네이밍 컨벤션을 정하는 것을 원문에서도 권장하고 있는 것 같아서  
버전 릴리즈마다 브랜치를 새로 파려고 한다. (물론 master에 merge되면 해당 브랜치 삭제)

결론은 !!!

### branch naming convention

- main (아맞다 이젠 메인이지)
- develop
- feature-(issue_number)
- release-(version_number)
- Hotfix-(Issue_number) (제발 쓸 일 없길 바라 ~~~) 



팟팅~!



## 추가

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



으악... 0.0.1 이후로 바꿔야할지 그냥 지금 바꿔야할지 ㅠ___ㅠ 아 이런거 싫어하는데..

[여기](https://gmlwjd9405.github.io/2018/05/11/types-of-git-branch.html)를 참고했다

