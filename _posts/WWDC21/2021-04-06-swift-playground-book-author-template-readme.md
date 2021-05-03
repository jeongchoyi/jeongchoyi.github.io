---
layout: post
title: "Swift Playground book author templte readme 파일 번역" 
date: 2021-04-06
category: read 
excerpt: ""
---

# Playground book author templte readme

> 를 번역하면서 정독해보자!

## **Playground Book Xcode Project**

### 개요

Playground Book Xcode project에 오신 것을 환영합니다!  
이 Xcode project로 두 가지를 만들 수 있어요.

* A playground book
* live view를 디버깅하기 위한 앱 

이를 지원하기 위해, 이 Xcode 프로젝트에는 다섯가지의 target이 있어요.



**PlaygroundBook**:  결과물로 playground book을 만듭니다. 

**BookCore**: 라이브 뷰 및 다른 작성자 전용 기능을 구현하는 보조 모듈인 *BookCore* 보조 모듈을 컴파일합니다.

**BookAPI**: 전체 book에 걸쳐 모든 사용자 코드에 자동으로 import되어 있는 보조 모듈인 *BookAPI* 보조 모듈을 컴파일합니다.

**UserModule**:  사용자가 코드를 적을 수 있는 빈 유저 모듈인 *UserModule* 유저 모듈을 컴파일합니다.

**LiveViewTestApp**: Swift Playgrounds에서 보여질 것과 비슷하게 live view를 보기 위해 Book_Sources 모듈을 사용하는 앱을 만듭니다. 



이 프로젝트는 Swift Playgrounds의 PlaygroundSupport, PlaygroundBluetooth 프레임워크를 포함합니다.  
그래서 BookCore, BookAPI, UserModule, and LiveViewTestApp target을 허용하고, 이 API들을 완전하게 사용할 수 있어요.  
이 템플릿에 포함된 위 프레임워크를 포함한 내용들은, 빌드를 하려면 Xcode 12.2 버전이 필요합니다.  
이 템플릿을 다른 버전의 Xcode와 함께 사용하려고 하면 빌드 오류가 발생할 수 있습니다.

playground book file format에 대해 더 정보가 필요하시면 *Swift Playgrounds authoring documentation* 을 참고하세요!

### First Steps

이 Xcode project를 시작하기 전에, 여러분의 playground book을 개인화 하기 위해 몇가지 바꿔야 할 것들이 있어요. 

BuildSettings.xcconfig (project navigator에서 “Config Files” 폴더 안에 있어요)을 열고, 아래 항목들을 수정해주세요!

* BUNDLE_IDENTIFIER_PREFIX 를 당신의 팀에 맞는 적절한 값으로 변경해주세요.
* PLAYGROUND_BOOK_FILE_NAME를 당신의 playground book에 맞는 적절한 값으로 변경해주세요.



또,  
Manifest.plist에서 PLAYGROUND_BOOK_CONTENT_VERSION 를 변경해 ContentVersion을 설정할 수 있어요. PLAYGROUND_BOOK_CONTENT_IDENTIFIER도 변경해 특정 ContentIdentifier를 설정할 수도 있죠! ( 기본 값은 BUNDLE_IDENTIFIER_PREFIX와 PLAYGROUND_BOOK_FILE_NAME를 기반으로 설정되어 있어요. )

ManifestPlist.strings (project navigator에서 “PlaygroundBook - PrivateResources” 폴더 안에 있어요) 파일을 열고 String 값들을 여러분의 playground book에 맞는 값으로 변경해주세요.

프로젝트 Editor를 열고, LiveViewTestApp target을 선택하고, Signing section에서 적당한 Team을 선택하세요. ( 이 과정은 여러분이 iOS 시뮬레이터에서만 테스트를 할 경우라면 건너뛰어도 괜찮습니다)



프로젝트 설정을 마치셨다면, 이제 PlaygroundBook target을 빌드할 수 있어요. 그러면 playground book이 결과물로 만들어집니다! (Project navigator에서 “Products” 폴더를 열고, 해당 파일을 우클릭해 “Show in Finder”를 눌러서도 접근할 수 있어요. 거기서, iPad로 에어드롭이나 다른 방법을 사용해 playground book을 공유한 다음  Swift Playgrounds를 실행할 수 있어요. Mac의 Swift Playgrounds를 사용하실거라면 그냥 더블클릭을 하면 됩니다.)



### Common Tasks

이 Xcode 프로젝트는 책이 올바르게 만들어질 수 있도록 매우 특별한 방법으로 구성되어 있는데요, Xcode와 On-disk입니다. 다음은 playground book을 만들 때 몇 가지 일반적인 작업을 수행하기 위한 가이드입니다.

**새 보조 모듈이나 유저 모듈 추가하기**

보조 모듈은 소스의 노출 없이 API를 playground book에 전달하는 Swift 모듈입니다. 유저 모듈은 사용자가 소스 모두를 100% 수정할 수 있는 Swift 모듈입니다. (보조 모듈과 유저 모듈에 대한 자세한 정보는 Swift Playgrounds authoring documentation을 참고하세요!)



보조 모듈이나 유저 모듈을 생성하기 위해, 디스크에 올바른 폴더 구조를 만들어야 합니다.  또 소스를 빌드하는 정적 라이브러리 target도 생성해야 하는데요, 코드 자동 완성과 실시간 issue같은 에디터 기능을 사용하기 위함입니다. 아래의 과정을 따라하시면 됩니다!

1. Project navigator에서, "PlaygroundBook" 폴더를 펼칩니다.
2. 보조 모듈을 생성하려고 한다면 "Modules" 폴더를,  
   유저 모듈을 생성하려고 한다면 "UserModules" 폴더를 펼칩니다. 
3. 위 폴더를 우클릭 (또는 ctrl+클릭)합니다.
4. context menu에서 "New Group"을 선택합니다.
5. 새로 생성된 폴더를 `ModuleName.playgroundmodule` 로 네이밍 해 주세요.  
   "ModuleName"은 여러분이 만들 모듈의 이름입니다.
6. 새로 생성된 폴더를 우클릭 (또는 ctrl+클릭) 합니다.
7. context menu에서 "New Group"을 선택합니다.
8. 새로 생성된 폴더의 이름을 `Sources`로 작성해줍니다.
9. 새로 생성된 폴더를 우클릭 (또는 ctrl+클릭) 합니다.
10. context menu에서 "New File..."을 선택하고, 새 Swift file을 생성합니다.
11. Project navigator에서 최상위 Xcode 프로젝트를 선택합니다.
12. Editor menu에서 "Add Target..."을 선택합니다.
13. assistant의 상단에서 iOS 플랫폼을 선택하고, “Static Library” 템플릿을 선택한 후 다음 버튼을 누릅니다.
14. "Product Name” 필드에 모듈의 이름을 넣고, “ModuleName”을 위에서(5번) 생성한 이름으로 맞춰주세요.
15. 다른 필드들을 작성하고 Finish 버튼을 누릅니다.
16. 프로젝트 에디터에서 해당 프로젝트를 선택하고, info 탭을 선택합니다.
17. Configurations outline view에서, 모든 build configuration을 펼칩니다.
18. 각 build configuration에서, 새로 추가된 target의 row에, "None"을 클릭하고 팝업 메뉴에서 “ModuleOverridingBuildSettings”을 선택합니다.
19. 프로젝트 에디터에서 새로 추가된 target을 선택하고, Build Settings 탭을 선택합니다.
20. 모든 빌드 설정을 보기 위해 필터에서 "All"을 선택합니다.
21. build setting을 선택하고, Edit menu를 열어 "Select All"을 선택합니다.
22. 키보드에서 Delete 키를 눌러 target 레벨에서 재정의된 모든 빌드 설정을 제거합니다.
23. Build Phases 탭을 선택합니다.
24. "Compile Sources" build phase를 펼칩니다.
25. "Compile Sources" build phase에 리스트된 모든 소스 파일을 제거합니다.
26. Project navigator에서, 위에서(10번) 새로 만든 swift 파일을 "Compile Sources" build phase로 드래그합니다.
27. Project navigator에서, 12-15단계에서 추가된 target 이름의 그룹을 찾습니다.  
    (i.e. “.playgroundbook” 확장자 **없이** “ModuleName” )
28. 그 그룹을 우클릭 (또는 ctrl+클릭) 합니다.
29. context menu에서 "Delete"를 선택합니다.
30. 확인창에서 "Move to Trash"를 선택해 소스 파일을 지웁니다.



이 과정을 따라하셨으면 잘 설정된, 독립적인 보조/유저 모듈이 만들어집니다. 위에서 방금 만든 모듈을 사용하신다면, 아래 항목들을 꼭 확인하세요.

* 보조/유저 모듈의 소스 파일들은 **꼭** 디스크 상의  
  "Modules(또는 UserModules) - ModuleName.playgroundmodule - Sources"  
  폴더에 있어야 해요.  
  만약 그 안에 없으면 그 파일들이 최종 playground book에 복사되지 않습니다 !!
* 모듈이 올바른 순서로 빌드되도록 하려면  
  Project editor에서 명시적으로 target dependencies를 지정해야 합니다.
* 해당 모듈을 LiveViewTestApp에서 사용하고 싶다면,  
  LiveViewTestApp에서 이 모듈으로 명시적 target dependencies를 설정해야 합니다.



### 보조/유저 모듈 지우기

보조/유저 모듈을 지우려면, 3가지를 삭제해야 하는데요,  

* 모듈을 빌드하는 라이브러리의 target
* Xcode project 모듈의 폴더
* 디스크상의 모듈 폴더

아래 과정을 따라하시면 됩니다.

1. Project navigator에서 최상단의 Xcode project를 선택합니다.
2. 삭제하고 싶은 module의 target을 선택합니다.
3. 모듈의 target을 우클릭합니다.
4. context menu에서 "Delete"를 선택합니다.
5. Project navigator에서 삭제하고 싶은 모듈의 폴더를 우클릭합니다.  
   (e.g. ModuleName.playgroundmodule)
6. context menu에서 "Delete"를 선택합니다.
7. 확인창에서 "Move to Trash"를 선택해 소스 파일을 지웁니다.
8. Project navigator에서 “PlaygroundBook” 폴더의 하위 항목인 “Supporting Content” 폴더를 펼칩니다.
9. “Modules” 이나 “UserModules” 폴더에서 삭제하고 싶은 모듈을 찾습니다.
10. 지우고 싶은 모듈을 우클릭합니다.
11. context menu에서 "Delete"를 선택합니다.



### 보조/유저 모듈의 이름 변경하기

보조/유저 모듈의 이름을 변경하려면, “ModuleName.playgroundmodule” 폴더명과 “ModuleName” 타겟명을 새로운 이름으로 변경하면 됩니다. (e.g. 각각 “NewModuleName.playgroundmodule” and “NewModuleName”)  
만약 book-level의 Manifest.plist에 있는 UserAutoImportedAuxiliaryModules 배열 내의 보조 모듈의 이름을 바꾸려면, 그 이름도 변경해야 합니다. 아니면 더이상의 변경은 필요하지 않아요.



### 보조/유저 모듈에 소스 추가하기 

이 프로젝트가 제대로 작동하려면, 소스 파일들은  
Xcode project에,  
모듈의 정적 라이브러리 target에,  
디스크의 ModuleName.playgroundmodule/Sources 폴더에  
추가되어야 합니다.

새 소스 파일을 추가하려면, 메뉴 창에서 *File > New > File…* 을 사용하거나  
“ModuleName.playgroundmodule” 폴더 안의 “Sources” 폴더를 우클릭해  *New File…*.을 선택하면 됩니다.

assistant 창에서, 적절한 템플릿을 고릅니다. (보통은 "Swift File"이나 "Cocoa Touch Class")  
assistant 창에서 새 파일을 저장할 때, 아래 항목들을 체크하세요!

* 파일이 저장될 곳이 ModuleName.playgroundmodule 폴더 내의 Sources 폴더인지  
  (“ModuleName” 은 여러분이 소스 파일을 추가할 모듈의 이름)
* 새 소스 파일이 ModuleName target에 추가되는지  
  (그리고 다른 target에는 추가되지 않는지도)

이전에 언급한 "소스" 그룹에 소스 파일을 추가하면 기본적으로 두 가지가 모두 참인 위치가 됩니다.

소스파일이 디스크상에서 올바르지 않은 Sources 폴더에 있으면, 최종 playground book에서 올바른 위치에 복사되지 않고, Swift Playgrounds에서 사용 불가능해집니다.

만약 소스 파일이 모듈의 정적 라이브러리 target의 member가 아니라면, Xcode에서 컴파일 되지 않습니다. 이는 구문 하이라이트(syntax highlighting), 코드 자동 완성 등 편집기 기능이 그 소스 파일에서 동작하지 않고, 그게 제공하는 API가 프로젝트의 다른 소스 파일에서 사용 불가능해진다는 뜻입니다.

> **참고**
> playground book은 Swift 소스 파일만 지원합니다. 이 Xcode 프로젝트는 그러한 사항을 강제하지는 않지만, 최종 playground book에 Swift가 아닌 소스 파일이 존재하면 Swift Playgrounds는 그것들을 무시합니다.



### Book-Level PrivateResources 추가하기

book-level PrivateResources 폴더에 내용을 추가하려면,  
Xcode 프로젝트에 리소스 파일을 추가하고,  
그것을 PlaygroundBook target의 “Copy Bundle Resources” build phase의 멤버로 설정해주세요.

그 파일은 필요시 컴파일 될 것이고 ( xib, 스토리보드, 에셋 카탈로그 및 기타 리소스 유형의 경우와 마찬가지로 )  
그 후에 playground book의 PrivateResources 폴더에 복사될거예요.



### Book-Level PublicResources 추가하기

이 Xcode 프로젝트는 기본적으로 book-level PublicResources를 지원하지 않아요.  
여러분의 책에 PublicResources 폴더를 추가하고 싶다면, 아래를 따라하세요!

1. Finder에서 PlaygroundBook 폴더 안에 PublicResources 폴더를 생성합니다.
2. Xcode에서 “PlaygroundBook” 폴더에 PublicResources를 folder reference로 추가합니다.  
   (group reference 아닙니다!)
3. PublicResources 폴더를 프로젝트 에디터의 PlaygroundBook target에  
   “Copy Book Contents” build phase로 추가합니다.

PrivateResources 와는 다르게, PublicResources 의 내용들은 복사만 되고, 컴파일 되지 않아요.  
만약 컴파일 리소스를 사용하려면, 꼭 PrivateResources로 사용하셔야 합니다.



### Chapters, Pages, Page-Level 내용 추가하기

이 Xcode 프로젝트는 여러분의 playground book 내의 chapter나 page를 수정하는데 제한적이예요.  
Chapters 폴더가 Xcode project에 폴더로 존재합니다.  
거기에 .playgroundchapter 패키지를 추가할 수 있고, 그러면 자동으로 최종 playground book에 복사될거예요.

챕터나 페이지를 추가할 때, 책이나 챕터의 Manifest.plist 파일도  
꼭 다른 새 챕터나 페이지를 참조하도록 수정해주세요.

이 Xcode 프로젝트는 챕터나 페이지 안에서는 구문 하이라이팅이나 자동완성, 실시간 이슈와같은 고급 에디터 기능을 지원하지 않아요. 이는 가능한 많은 코드가 보조 모듈에 포함되어서, 챕터랑 페이지에는 가능한 적은 코드가 있게 하는 것을 추천하기 때문이예요.



### Live View 테스트하기

이 Xcode 프로젝트는 여러분의 playground book의 live view를 테스트하는 것을 지원하고 있습니다. 이 테스트 지원은 여러분의 playground book의 live view가 BookCore 보조 모듈에 구현되어있다는 것을 가정해요. 만약 다른 곳에 구현되어 있다면 (i.e. page의 Contents.swift 나 LiveView.swift 파일) 이 매커니즘으로는 테스트 하기 쉽지 않을거예요.

여러분의 live view를 테스트 하고 싶다면, LiveViewTestApp 앱을 실행하세요. 이 앱은 iPad, iOS 시뮬레이터, Mac Catalyst 앱에서 동작합니다. 이 앱은 Swift Playgrounds가 보여줄 비슷한 방식으로 live view를 보여줄 수 있어요. 특히 LiveViewTestApp은 Playground Support 프레임워크에서 노출되는 라이브 뷰 safe area 레이아웃 가이드를 올바르게 구성합니다.

여러분의 live view를 구성하기 위해, AppDelegate.swift에서 setUpLiveView() 메서드를 구현하세요. 이 메서드는 live view로 사용될 준비가 된 UIView나 UIViewController를 반환해야 합니다.

기본적으로, LiveViewTestApp는 여러분의 live view를 전체 화면으로 실행합니다. LiveViewTestApp는 또한 여러분의 live view를 side-by-side view로도 실행할 수 있어요. ( 마치 Swift Playground에서 소스코드 에디터가 live view의 옆이나 아래에 있는 것 처럼요! ) 이렇게 하려면, AppDelegate.swift의 liveViewConfiguration 프로퍼티가 .fullScreen 대신 .sideBySide를 반환하게 하면 됩니다.

