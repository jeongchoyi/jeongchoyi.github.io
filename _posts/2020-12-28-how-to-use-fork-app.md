---
layout: post
title: "아요모모를 위한 fork app 사용법" 
date: 2020-12-28
category: read 
excerpt: ""

---

# 아요모모를 위한 fork app 사용법

## 길어질까봐 만들어둔 목차

[Repository Clone 하기](#clone-repository-anchor)<a id="clone-repository"></a>

[Branch Tracking, 생성하기](#make-branch-anchor)<a id="make-branch"></a>

[Commit 하기](#commit-anchor)<a id="commit"></a>

[Commit 취소하기](#revert-commit-anchor)<a id="revert-commit"></a>

[Conflict 해결하기](#resolve-conflict-anchor)<a id="resolve-conflict"></a>

## Repository Clone 하기[](#clone-repository)<a id="clone-repository-anchor"></a>

![image](https://user-images.githubusercontent.com/28949235/103214215-32e65080-4953-11eb-99c3-950694555307.png) 

Repo Fork 뜨고, 복사 긔긔

![image](https://user-images.githubusercontent.com/28949235/103214063-d5ea9a80-4952-11eb-91b9-9d4b7c3728d9.png)

Fork에 들어와서 `File`-`Clone` 클릭 !  

![image](https://user-images.githubusercontent.com/28949235/103214272-57422d00-4953-11eb-9793-2b8009f0ffab.png)

`Clone` 버튼 누르면 끝 >_<~~~

## Branch Tracking, 생성하기[](#make-branch)<a id="make-branch-anchor"></a>

![image-20201228213615276](/Users/choyi/Library/Application Support/typora-user-images/image-20201228213615276.png)

develop 브랜치 저 연두색 박스를 더블클릭!

![image-20201228213702558](https://user-images.githubusercontent.com/28949235/103214929-29f67e80-4955-11eb-8755-7d759bbddda4.png)

Track 해주면 왼쪽에 추가돼요 >> ![image](https://user-images.githubusercontent.com/28949235/103214882-09c6bf80-4955-11eb-9505-32b7fc052e38.png)

develop을 default 로 쓸거라! 모든 브랜치를 feature별로 팔 때에는 develop에서 파줍니다.

![image](https://user-images.githubusercontent.com/28949235/103214845-f61b5900-4954-11eb-9628-a045a49ff363.png)

## Commit[](#commit)<a id="commit-anchor"></a>

여러 파일을 생성하고, 작업 후 `아 맞다 커밋 안했다;` 라는 생각이 들 때 !!  
괜찮슴다~~ 하나하나 한줄한줄 stage하는 게 가능함다 ~~  
![image](https://user-images.githubusercontent.com/28949235/103210276-928b2e80-4948-11eb-97c8-26e601093f5c.png)

왼쪽 위 Local Changes에 들어가면 현재 브랜치에서 변경된 파일을 커밋 전에 확인할 수 있는데요,

![image](https://user-images.githubusercontent.com/28949235/103210319-ac2c7600-4948-11eb-8550-3772abd73b0b.png)

요렇게 Unstaged, Staged로 구분이 되어 있습니다.  
우리가 커밋하고 싶은 파일들만 Stage 해주면 돼요!

저는 APIConstants, AppConstants를 따로 커밋하고 싶은데, 우선 APIConstants만 커밋 해 봅시다.  
그러면 APIConstants만 Staged에 올려주면 되겠죠?  
![image](https://user-images.githubusercontent.com/28949235/103210462-0d544980-4949-11eb-893d-6d3acff33314.png)

해당 파일을 클릭하고 Unstage를 눌러줍시다.

![image](https://user-images.githubusercontent.com/28949235/103210500-252bcd80-4949-11eb-8a29-d5a05087e575.png)

그럼 이렇게 APIConstants만 남아서, 이 파일만 커밋을 할 수가 있어요~!  
그럼 이제 바로 커밋 버튼 누르면 되냐~! 아닙니다 ~!  
저는 파일을 "생성" 해서 작업했기 때문에, .pbxproj 파일에 해당 파일의 경로가 추가되어 있어요.  
요놈까지 같이 커밋해줘야 하는데...  
![image](https://user-images.githubusercontent.com/28949235/103210495-2230dd00-4949-11eb-82e4-98066fa2b239.png)

ㄱ- AppConstants랑 APIConstants를 중간에 커밋 안하고 한번에 작업해버려서  
한 파일에 이렇게 두개 파일의 경로가 모두 추가되어 있네요,, 그렇다고 걍 두개 다 올려버리긴 그렇죠,,  
APIConstants에서 File Changes가 안 예쁘니까 ^.^ 

코드에 마우스를 갖다대면 해당 코드 블럭이 자동으로 잡히는데요,  
![image](https://user-images.githubusercontent.com/28949235/103210625-85bb0a80-4949-11eb-85c0-afcf0e518a40.png)

요 코드 블럭 내에서 저희는 APIConstants 경로를 담고 있는 딱 한줄만 Stage하고 싶으니까  
해당 줄을 드래그 해 줍니다.

![image](https://user-images.githubusercontent.com/28949235/103210611-7d62cf80-4949-11eb-80e2-4d130978a7f5.png)

요로코롬.. 그러면 해당 line만 코드 블럭이 잡히고 Stage 버튼을 누를 수 있죠 ! 눌러줍니다

![image](https://user-images.githubusercontent.com/28949235/103210661-adaa6e00-4949-11eb-8c58-46d305e55a54.png)

project.pbxproj 가 stage 되었어요. 여기는 우리가 방금 Stage한 딱 한 줄만 ! 포함되어 있답니당  
그러면 Unstage된 project.pbxproj 파일로 다시 가서,  
APIConstants를 문제 없이 커밋하기 위한 줄들을 다 Stage 시켜줘야겠죠!  
APIConstants.swift 파일 경로 뿐만 아니라, APIConstants.swift 파일이 있는 Constants 폴더의 경로도요!

![image](https://user-images.githubusercontent.com/28949235/103210771-fd893500-4949-11eb-8eb2-e69f73159131.png)

Stage 해줍니다. 아래로 내려가면서 APIConstants 파일, Constants 폴더와 관련 있는 줄들은 다 Stage !!  
(나중에 AppConstants할 때에는 Constants 폴더의 경로는 변경이 없으니까 초록줄로 안 뜨겠쬬 ~!)

![image](https://user-images.githubusercontent.com/28949235/103210835-26a9c580-494a-11eb-846b-f0495a71e689.png)

APIConstants, Constants 폴더와 관련된 건 다 Stage 해줘서 Unstage에는 AppConstants만 남은 모습입니동
( 위에 Unstaged에 APIConstants 있는거 제 실수입니다.. 원래 없어야 정상임.. ㅈㅅㅈㅅ..)

![image](https://user-images.githubusercontent.com/28949235/103210942-58bb2780-494a-11eb-937f-a4e9e04369e4.png)

Commit message 쓰고 Commit 버튼 누르면 끄읏 ^___^



## Commit 취소하기 [](#revert-commit)<a id="revert-commit-anchor"></a>

push 전이라면 ~

![image](https://user-images.githubusercontent.com/28949235/103211144-b9e2fb00-494a-11eb-9eb6-094d95beb235.png)



## Conflict 해결하기  [](#resolve-conflict)<a id="resolve-conflict-anchor"></a>

![image](https://user-images.githubusercontent.com/28949235/103211272-16461a80-494b-11eb-96b3-d7f74a279c0c.png)

Conflict가 난 모습입니다.. ㄷㄷ

![image](https://user-images.githubusercontent.com/28949235/103211306-2c53db00-494b-11eb-84ad-a83bf2e61ca6.png)

한 파일로 그냥 덮어쓰려면 버릴 파일 쪽 체크를 해제하면 됩니다.  
근데 보통 이럴 일은 잘 없고.. 하나하나 지워가며 해결해줘야 하죠.. 그럴 땐  
![image](https://user-images.githubusercontent.com/28949235/103211369-5c02e300-494b-11eb-97f7-a7a44cb22365.png)

Merge in Fork를 눌러봅시다.

![image](https://user-images.githubusercontent.com/28949235/103211408-763cc100-494b-11eb-8082-a2d8bbd68d8d.png)

요렇게 Left, Right를 선택할 수 있답니다 ~!! cool~!

![image](https://user-images.githubusercontent.com/28949235/103211455-99677080-494b-11eb-8bcb-4d3d1886979b.png)

모든 conflicts가 해결되면 왼쪽 아래에 이렇게 뜨고요, Resolve 버튼을 눌러주면 됩니다!

## Push, Fetch, Pull

![image](https://user-images.githubusercontent.com/28949235/103214960-47c3e380-4955-11eb-9a89-696ca255263e.png)

여기서 하면 돼요! !! 이건 내가 할때 추가해놓겠삼 