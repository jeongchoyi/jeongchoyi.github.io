---
ㅁㅔlayout: post
title: "협업 시작 전 Git Organization 및 repository 파기" 
date: 2020-12-27
category: read 
excerpt: ""

---

앱잼 진행 사항들을 종종 기록해 두려고 한다~! 처음부터~~~

# 협업 시작 전 Git Organization 및 repository 파기

협업을 위한 repository 초기 설정~~!

### Dependabot alerts - `Disable` 해두기

![image](https://user-images.githubusercontent.com/28949235/103187616-5801a180-4908-11eb-93a3-eeb2810ea069.png)

보기 싫어서 꺼둠,, ^.^ㅎㅎ

### Branch protection rules

![image](https://user-images.githubusercontent.com/28949235/103187849-6dc39680-4909-11eb-8eed-60e7a6347b85.png)

`main`, `develop` 브랜치에 모두 설정해줬다. 브랜치 프로텍션을 설정해두는 이유!  
아래와 같은 상황이 **일어나지 않게** 해 준다.

1. force pushing
2. 브랜치 삭제
3. `main` 브랜치에 직접 push
4. 내가 PR 날린거에 본인이 approve
5. 아무도 approve 하지 않았는데 merge
6. approve 이후에 추가로 commit 후 merge



### main branch setting

![image](https://user-images.githubusercontent.com/28949235/103189830-e6c6ec00-4911-11eb-9fc6-345d6af584b0.png)

> PR merge 위한 최소 approve 갯수 : 2  
> approve 이후 새 commit이 있으면 기존 approve가 취소됨  
> 리뷰를 꼭 받아야하는 코드 owner를 지정 가능  
> 관리자도 예외 없이 적용



### develop branch setting

![image](https://user-images.githubusercontent.com/28949235/103194055-a96a5a80-4921-11eb-9195-5541ae42b75e.png)



세팅했다고 바로 브랜치 만들어지는 거 아니니까 브랜치는 따로 만들어 줘야 함!

