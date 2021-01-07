---
layout: post
title: "pbxproj, plist conflict 해결하기" 
date: 2020-11-27
category: read 
excerpt: ""

---

# 각자 작업물 main 브랜치로 merge - pbxproj, plist conflict 해결하기

> 아래는 SOPT 27기 합동세미나 issue에 docs로 작성한 내용 복사해온 것!

merge를 제가 아무것도 안 알려주고 짠! 머지 했습니다! 하기보다는
conflict 과정을 공유하는 게 좋을 것 같아서 작성합니다~

### pbxproj, plist conflict 해결하기

제가 기본 폴더링을 해 두긴 했지만, 각자 새 파일이나 폴더를 만드는 과정에서
`project.pbxproj` 파일에 각 파일이나 폴더의 경로가 추가가 되는데요,

각자 다른 파일을 만들고, 순차적으로 누가 PR날리면 풀받고 작업하고 내가 PR날리면 다같이 풀받고 작업하고 이게 아니기 때문에
 conflict가 자연스레 발생하는 상황입니다.

![image](https://user-images.githubusercontent.com/28949235/100407300-63647180-30ab-11eb-864e-4c8844b990f6.png)
요렇게 말이죠..
(info.plist의 conflict는 저희가 simulator를 돌리기 위해 main storyboard를 각자 다르게 설정했기 때문이예요! [아래](#plist-anchor)<a id="plist"></a>를 참고,,)
Resolve Conflicts를 눌러서 conflict를 해결해 봅시다

![image](https://user-images.githubusercontent.com/28949235/100407368-8f7ff280-30ab-11eb-86d0-73a198e5058d.png)
들어가면 이런식으로 컨플릭이 난 부분이 여러 군데 존재하는데요,
```
<<<<<<< yoonah
		ED3CBDCF255F94C9003572C6 /* PreViewSB.storyboard in Resources */ = {isa = PBXBuildFile; fileRef = ED3CBDCE255F94C9003572C6 /* PreViewSB.storyboard */; };
		ED3CBDD3255FA2FC003572C6 /* PreViewVC.swift in Sources */ = {isa = PBXBuildFile; fileRef = ED3CBDD2255FA2FB003572C6 /* PreViewVC.swift */; };
=======
		562F684D255F94DD00C50C97 /* MindMapSB.storyboard in Resources */ = {isa = PBXBuildFile; fileRef = 562F684C255F94DD00C50C97 /* MindMapSB.storyboard */; };
		562F6851255F982100C50C97 /* MindMapVC.swift in Sources */ = {isa = PBXBuildFile; fileRef = 562F6850255F982100C50C97 /* MindMapVC.swift */; };
>>>>>>> main
```

/* */ 요 블럭 안에 파일명을 보면
yoonah 브랜치에서는 PreViewSB, PreViewVC
main 브랜치에서는 MindMapSB, MindMapVC
가 존재하죠!
이 4가지는 모두 존재해야하고, 겹치는 파일들이 아니기 때문에 남겨놓아야합니다.
(잘못해서 삭제해버리면 프로젝트가 안 열리거나, 프로젝트가 저 파일을 못 찾으니 지울때는 꼭 유의하세요!~)

그러므로 
`<<<<<<< yoonah`, `=======`, `>>>>>>> main`
이 부분을 지워서 그냥 다 남겨주시면 됩니다.

![image](https://user-images.githubusercontent.com/28949235/100407553-0c12d100-30ac-11eb-96d0-b177fa8ef6f9.png)
conflict가 사라진 모습입니다.
이런식으로 다 해결해주신 후
![image](https://user-images.githubusercontent.com/28949235/100407617-349acb00-30ac-11eb-99ca-99334dca7158.png)
요거 누르면 됩니다.

### plist conflict 해결하기[](#plist)<a id="plist-anchor"></a>
![image](https://user-images.githubusercontent.com/28949235/100407699-70ce2b80-30ac-11eb-8658-08090d50e51c.png)
plist 에서는 이런 conflict가 났는데요, 이 부분은

![image](https://user-images.githubusercontent.com/28949235/100407717-7cb9ed80-30ac-11eb-8f54-05e85f23b1d9.png)
이 부분의 값이 달라서 발생하는 conflict입니다.

위에 pbxproj 의 conflict를 해결할 때와는 다르게, 둘 중 하나만 존재해야하는 상황이죠?
지금 같은 경우 둘 중 아무거나 해도 크게는 상관없지만...(스토리보드 이름이야 각자 작업할때 바꾸면 되니까요)
프로젝트 내 파일이나.. 폴더 등등 같은 경우에는 팀원의 작업물이 통째로 사라져버릴 수 있기 때문에
이렇게 하나를 지워야 하는 경우 꼭 조심해주셔야 합니다!

저는 우선 yoonah 브랜치에 있던 내용으로 남기고 main 브랜치 내용을 지워서 merge할게요.
(어차피 서버 합동세미나때 listPage로 첫화면 바꿔줘야하는데, 그 전에 각자 작업할 땐 각자 작업하는 storyboard로 지정해야하므로)

똑같이 Mark as Resolved 버튼을 눌러주신 후,
commit merge 버튼을 눌러서 merge를 완료해주시면 됩니다~
![image](https://user-images.githubusercontent.com/28949235/100407966-26997a00-30ad-11eb-9ce2-e4d8b88966ea.png)

글은 길지만.. 그렇게 어렵지 않으니 앱잼때 conflict날 때 당황하지 말고 해결합시다 ㅎ_ㅎ~~~