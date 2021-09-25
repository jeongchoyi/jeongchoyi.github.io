---
layout: post
title: "UINavigationController, UINavigationBar 구조" 
date: 2021-09-25
category: read 
excerpt: ""

---

# UINavigationController

[Docs](https://developer.apple.com/documentation/uikit/uinavigationcontroller)

>  계층적인 navigating 컨텐츠를 위한 스택 기반 체계를 정의하는 container view controller

### stack

UINavigation Controller: 네비게이션 인터페이스 내, 하나 또는 그 이상의 뷰 컨트롤러를 관리하는 container 뷰컨!  
이런 종류의 인터페이스에서는, 한 번에 한 뷰 컨트롤러만 보여질 수 있다.  
( 화면에서 한 뷰컨이 push된다는 건, 그 뷰컨이 다른 뷰컨을 가린다는 뜻 ! )  
네비게이션 바의 back button을 누르면 최상단의 뷰컨을 지우고, 그래서 그 아래의 뷰컨이 드러난다.

![image](https://user-images.githubusercontent.com/28949235/134767495-105f73fd-67f4-4486-8c51-dc414a534a04.png)

navigation controller 오브젝트는 child 뷰컨들을 순서가 있는 배열로 관리하는데,  
그게 바로 **navigation stack**이다.  
배열의 첫번째 뷰컨이 root 뷰컨이고, stack의 bottom을 나타낸다.  
배열의 마지막 뷰컨은 stack의 최상단이고, 현재 보여지고 있는 뷰컨을 나타낸다.

navigation controller는 최상단의 navigation bar, 하단의 toolbar(optional)를 관리한다.  
**navigation bar**는 항상 떠있고, navigation controller 자체에서 관리한다.  
navigation controller는 child 뷰컨에서 제공하는 내용을 사용해서 navigation bar를 업데이트한다.  

### delegate

navigation controller는 자신의 `delegate` 객체를 사용해서 동작을 조정한다.  
이 delegate 객체로 아래의 것들을 할 수 있다.  

1. 뷰컨을 push, pop 하는 함수를 오버라이딩하기
2. 애니메이션 transition을 커스텀하기
3. navigation 인터페이스의 preferred orientation, 방향을 지정하기

delegate 객체는 무조건 `UINavigationControllerDelegate` protocol을 따라야 한다.

> UINavigationController가 관리하는 객체들

![image](https://user-images.githubusercontent.com/28949235/134767731-894afc84-2618-431c-a3ad-b23f1128ff41.png)

### Navigation Controller Views

Navigation Controller는 container view controller다.  
그 말은, 자신의 내부에 다른 뷰컨의 내용을 embed 한다는 뜻이다.  
Navigation Controller의 view는 `view` 프로퍼티로 접근할 수 있다.  
그 뷰는 navigation bar, optional toolbar, 최상단 뷰컨에 대응하는 content view를 통합한다.

<img src="https://user-images.githubusercontent.com/28949235/134767849-a17b55bb-3b30-4650-8e5f-c5d63a344d39.png" alt="image" width=500 />

위 그림과 같이 합쳐진다.

![image](https://user-images.githubusercontent.com/28949235/134767923-59e80885-e600-4782-adb5-6109e51154a3.png)

실제로 view hierarchy를 보면  
맨 뒤 부터 Window - Tab bar view - Navigation view - MainViewController 순이다.

Navigation Controller는 bar, toolbar의  
creation, configuration, display를 관리한다.  
navigation bar가 어떻게 보일지와 관련된 것들을 관리할 수 있는 권한이 있지만,  
**frame, bounds, alpha 값은 직접적으로 바꾸면 절대 !! 안된다.**

navigation bar를 보이게, 숨기게 하고 싶으면,  
`isNavigationBarHidden` 프로퍼티나 `setNavigationBarHidden(_:animated:)` 메소드를 사용하면 된다.

navigation controller는   
navigation stack상의 뷰컨들과 관련된 navigation item 객체(`UINavigationItem` 클래스의 인스턴스)를 사용해서  
동적으로 navigation bar의 컨텐츠를 빌드한다.

navigation bar의 전체적인 모습을 커스텀하려면, **`UIAppearance` API를 사용하면 된다.**  
navigation bar의 내용을 변경하려면, 뷰컨에 navigation item을 만들어야 한다.

### Navigation Bar 업데이트 하기

최상단의 뷰컨이 바뀔때마다, navigation controller가 그에 따라 navigation bar를 업데이트한다.  
특히, navigation controller가 
left, middle, right 3개의 navigation bar 포지션 각각에 표시되는 bar button item을 업데이트한다.  
(bar button item은 `UIBarButtonItem` 클래스의 인스턴스)  

`tintColor`는 bar item의 색을 바꿀 때,  
`barTintColor` 프로퍼티는 bar 자체의 색을 바꿀 때 사용한다.

Navigation bar는 현재 보여지는 뷰컨에서 tint color를 상속받지 **않는다**.



# UINavigationBar

[Docs](https://developer.apple.com/documentation/uikit/uinavigationbar)

<img src="https://user-images.githubusercontent.com/28949235/134769062-ac88e345-fbd3-436b-9670-5a3c8c46277f.png" alt="image" width=500 />

주된 컴포넌트는 left(back) button, center title, right button(optional) 이다.  
 navigation bar를 사용하는 데에는 두 가지 방법이 있는데,  

1. Navigation Controller 객체랑 같이 사용하기
2. 단독으로 사용하기

### Navigation Controller 객체랑 같이 사용하기

navigation controller를 사용할 때 navigation bar를 조정하려면,  
아래의 단계를 따라야 한다.

* 인터페이스 빌더나 코드로 navigation controller를 만든다.
* `UINavigationController`객체의 `navigationBar` 프로퍼티를 사용해서  
  navigation bar의 모습을 구성한다.
* navigation controller의 스택에 push하는 뷰컨 각각의  
  `title`, `navigationItem`을 설정해서 navigation bar의 내용을 제어한다.

### 단독으로 사용하기

navigation controller없이 단독으로 navigation bar를 사용할 수도 있는데,  
아래의 단계를 따라야 한다.

* navigation bar의 위치를 지정하기 위해 오토레이아웃을 설정한다.
* 초기 title을 설정하기 위해 root navigation item을 생성한다.
* navigation bar에 대한 사용자 인터랙션을 다루기 위해 delegate를 구성한다.
* navigation bar의 모습을 커스텀한다
* 사용자가 계층적 화면을 navigate할 때 관련 navigation item들을 push, pop하도록 앱을 구성한다

### Navigation Bar를 따로 추가해서 쓴다면 그 이유는!?

거의 모든 경우에서 Navigation Controller의 부분으로 navigation bar를 사용한다.
 근데, navigation bar UI를 사용하고 싶고,
 content navigation에 대한 고유한 접근 방식을 구현해야 하는 상황이 있다고 한다.
 그런 상황에서 navigation bar를 단독으로 사용할 수 있는 것!
