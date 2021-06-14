---
layout: post
title: "VC, View Life Cycle 함수들 알아보기" 
date: 2021-06-14
category: read 
excerpt: ""

---

# VC, View Life Cycle 함수들 알아보기

[목차](https://iamcho2.github.io/2021/06/08/master-of-ios-life-cycle)

###  📺 예고 다시보기

 🤷‍♀️ 3단계가 있는거 알겠고, 그 안에 무슨 함수들이 있는지 알겠어.  
근데 **언제 어디서 쓰는건데 !? 내가 오버라이딩 해서 써도 되는거냐고!!!**  
에 대한 답을 찾으러 떠나봅시다...... 💪 ( ◠‿◠ )



## 서론

[저번 포스팅](https://iamcho2.github.io/2021/06/09/view-viewcontroller-layout-cycle-with-render-loop)에서는 VC + View Life Cycle 함수들의 호출 순서와  
Render Loop의 3가지 단계를 알아봤었다.

![qeefqe](https://user-images.githubusercontent.com/28949235/121914177-fecde300-cd6c-11eb-89fe-388c1aafaa31.png)

요 이미지까지 보고 끝났었는데, 함수들을 하나하나 조금 더 자세히 알아볼 것!

> init()은 생성자니까 건너뛰고 ~

### loadView()

[📑 Apple Docs](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621454-loadview)   

* **뭐 하는 함수인데?**
  * 뷰 컨트롤러가 그 뷰 컨트롤러의 메인 뷰(`self.view`)를 로드할 때 호출
  * 뷰 컨트롤러의 기본 뷰를 커스텀 뷰로 사용하고자 할 때 유용
  * IB 외에 코드로 뷰 컨트롤러를 만들었을 때 사용
    * IB로 뷰 컨트롤러를 만들면 자동으로 뷰가 생성되어 필요 없음

* **오버라이드 가능 여부** : **O**

  * **오버라이딩 목적** : manually하게 고유한 뷰(다른 VC와 공유하지 않는)를 만들고 싶을 때

* **주의사항**

  * 직접 호출 금지
  * 해당 함수를 구현할 때 super 호출 금지 (뷰를 직접 만들어야 함)
  * Interface Builder를 사용한다면 해당 함수 오버라이딩 X (필요가 없음)

  * `UIView`를 뷰 컨트롤러의 기본 뷰로 사용하고,  
    그 위에 무언가 얹어서 쓰거나 뷰가 생성된 이후에 어떤 세팅을 해서 사용하고 싶다면  
    `loadView()`가 아니라 `viewDidLoad()`를 사용



### viewDidLoad()

[📑 Apple Docs](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621495-viewdidload)   

* **뭐 하는 함수인데?**
  * 뷰 컨트롤러가 그 뷰 컨트롤러의 메인 뷰가 메모리에 로드되었을 때 호출

* **오버라이드 가능 여부** : **O**
  * **오버라이딩 목적** : 로드 된 뷰에 추가적인 작업을 수행하고 싶을 때
* **주의사항**
  * 뷰 계층이 nib 파일에서 로드되었는지,  
    `loadView()` 에서 프로그래밍 방식으로 생성되었는지에 관계없이 호출됨
  * super를 호출해야 함



### viewWillAppear()

[📑 Apple Docs](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621510-viewwillappear)   

* **뭐 하는 함수인데?**
  * 뷰 컨트롤러가 뷰 hierarchy에 추가되기 직전에 호출

* **오버라이드 가능 여부** : **O**

  * **오버라이딩 목적** : 뷰를 나타내는 것 관련해서 사용자 정의 task를 수행할 때  
    ex: 보여질 뷰의 방향에 따라 상단바의 방향 변경하기
  * view가 스크린에 보이기 직전에 실행되어야 할 코드 작성

* **주의사항**

  * 오버라이딩 할 때, `super`를 호출해야 함

  * 특정 상황 (ex: popover 에서 뷰컨을 호출하고 다시 popover가 있는 뷰컨으로 돌아올 때)에서는  
    `viewWillAppear()`가 호출되지 않음



---

> 💡 여기부턴 Render Loop의 **Update Constraints** 단계

### updateConstraints()

[📑 Apple Docs](https://developer.apple.com/documentation/uikit/uiview/1622512-updateconstraints)   

* **뭐 하는 함수인데?**
  * Render Loop의 Update Constraints 단계에서 뷰의 제약조건을 업데이트 해 줌

* **오버라이드 가능 여부** : **O**
  * **오버라이딩 목적** : 제약 조건 변경을 최적화할 때
* **주의사항**
  * 직접 호출 금지 (시스템이 자동으로 호출해 줌)
  * 모든 제약 조건을 이 함수를 통해 변경하는 것은 절대 아님.  
    ex: 버튼 탭에 대한 응답으로 제약 조건을 변경하려면 버튼의 동작 메서드에서 직접 변경하는게 가장 좋음!
  * 영향을 미치는 변경이 발생한 직후 제약 조건을 업데이트하는 것이 거의 항상 깨끗하고 쉽다 - apple
  * 제약 조건을 변경하는 것이 너무 느리거나 뷰가 많은 중복 변경을 생성하는 경우에만 오버라이드 할 것!  
    
  * 변경을 예약하고 싶다면 **뷰에서** `setNeedsUpdateConstraints()`를 호출할 것  
    이렇게 하면 Layout 단계가 끝난 후 `updateConstraints()`를 호출한다!  
    ( 커스텀 뷰의 속성이 변경되지 않을 때 콘텐츠에 필요한 모든 제약 조건이 적용되었는지 확인할 수 있음)
  * 모든 제약사항을 다 비활성화 시키고 필요한 것을 활성화 시키는 식으로 구현하지 **마라**!! 비효율적이다!  
    바꿔야 하는 것만 바꿔라. - apple
  * 구현의 **마지막에 `super`를 호출**할 것  
    ( 저번 포스팅에서도 봤듯, 이 함수는 호출되는 방향이 Bottom-up 방향이니까!)



> 이 순서에서 **intrinsicContentSize**를 참조 ([알아보기](https://iamcho2.github.io/2021/06/15/intrinsicContentSize))



### updateViewConstraints()

> UIView의 `updateConstraints()` 의 뷰 컨트롤러 버전

[📑 Apple Docs](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621379-updateviewconstraints)   

* **뭐 하는 함수인데?**
  * 뷰 컨트롤러의 뷰가 뷰의 제약사항을 업데이트 해야 할 때 호출됨

* **오버라이드 가능 여부** : **O**
  * 오버라이딩 목적 : 제약 조건 변경을 최적화할 때
* **주의사항**
  * `setNeedsUpdateConstraints()`, `updateConstraintsIfNeeded()` 가 있다면 그 후에 호출됨  
    (`setNeedsUpdateConstraints()`가 결국 `updateViewConstraints()`를 호출)
  * 다른 주의사항은 `updateConstraints()`와 동일!



---

> 💡 여기부턴 Render Loop의 **Layout** 단계

### viewWillLayoutSubviews()

[📑 Apple Docs](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621437-viewwilllayoutsubviews)   

* **뭐 하는 함수인데?**
  * 뷰 컨트롤러에게 "이제 니 view가 subview들 배치할거래!"라고 알려주기 위해 호출됨
  * 뷰의 경계가 변경되면 뷰는 하위 뷰의 위치를 조정하는데,  
    그 전에 뷰 컨트롤러에게 알려주는 것.

* **오버라이드 가능 여부** : **O**
  * 오버라이딩 목적 : 뷰가 그 뷰의 서브뷰들을 배치하기 전에 변경사항을 주기 위해
  * 변경사항?
    * 뷰들을 추가하거나 제거하기
    * 뷰의 크기나 위치를 업데이트
    * 레이아웃 constraint를 업데이트
    * 뷰와 관련된 기타 프로퍼티들을 업데이트
* **주의사항**
  * 이 함수에 추가적인 구현 안해주면 기본 함수는 아무 동작을 하지 않음  
    (기본적으로 구현되어 있는 게 없음)



### layoutSubviews()

[📑 Apple Docs](https://developer.apple.com/documentation/uikit/uiview/1622482-layoutsubviews)   

* **뭐 하는 함수인데?**
  * Render Loop의 Layout 단계에서 제약조건을 바탕으로 레이아웃(뷰의 프레임 수치 값)을 업데이트 

* **오버라이드 가능 여부** : **O**
  * 오버라이딩 목적 :  
    서브클래스가 그 하위 뷰들에 대해 더 정확한 레이아웃을 수행하고 싶을 때 서브클래스에서 오버라이드
* **주의사항**
  * 직접 호출 금지
  * 하위 뷰의 autoresizing 및 제약사항 기반 동작이 원하는 동작을 제공하지 않는 경우에만 오버라이드 해야 함
  * 다음 draw 단계 전에 레이아웃을 강제로 업데이트 하고 싶으면 `setNeedsLayout()` 호출,  
    근데 지금 당장!!! 업데이트 하고 싶으면 `layoutIfNeeded()` 호출



### viewDidLayoutSubviews()

[📑 Apple Docs](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621398-viewdidlayoutsubviews)   

* **뭐 하는 함수인데?**
  * 뷰 컨트롤러에게 "방금 니 view가 subview들 배치했대!"라고 알려주기 위해 호출됨
  * 뷰의 경계가 변경되면 뷰는 하위 뷰의 위치를 조정하는데,  
    그 후에 뷰 컨트롤러에게 알려주는 것.

* **오버라이드 가능 여부** : **O**
  * 오버라이딩 목적 : 뷰가 그 뷰의 서브뷰들을 배치한 후에 변경사항을 주기 위해
  * 변경사항?
    * 다른 뷰들의 내용 업데이트
    * 뷰들의 크기나 위치를 최종적으로 조정
    * 테이블의 데이터를 reload
* **주의사항**
  * 이 함수가 호출되었다고 해서 하위 뷰들의 개별적인 레이아웃들이 다 조정되었다는 것은 아님!  
    각 하위 뷰들은 각자의 레이아웃을 조정해야 됨
  * 이 함수에 추가적인 구현 안해주면 기본 함수는 아무 동작을 하지 않음  
    (기본적으로 구현되어 있는 게 없음)



---

> 💡 여기부턴 Render Loop의 **Display** 단계

### draw()

[📑 Apple Docs](https://developer.apple.com/documentation/uikit/uiview/1622529-draw)   

* **뭐 하는 함수인데?**
  * 뷰에서 직사각형으로 특정된 영역에 한해 뷰를 다시 그리는 등 업데이트 할 때 호  
    (매개변수로 CGRect를 받음)

* **오버라이드 가능 여부** : **O**
  * 오버라이딩 목적 :  
    CoreGraphics나 UIKit같은 기술을 사용해서 뷰를 그리는 서브 클래스에서 drawing code를 구현하기 위해 
* **주의사항**
  * 직접 호출 금지  
    `setNeedsDisplay()` 같은 함수를 호출해서 다시 draw가 필요함을 시스템에 알리고  
    다음 cycle에 `draw()`가 호출되도록 해야 함
  * 기본적으로 구현되어 있는 게 없음
  * 뷰가 다른 방법으로 내용을 설정한다면 오버라이딩 할 필요 없음  
    (ex: 뷰가 background color만 보여준다거나 할 때)
  * 현재 뷰가 UIView를 상속받고 있다면 `super` 호출 필요 없음  



---

### viewDidAppear()

[📑 Apple Docs](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621423-viewdidappear)   

* **뭐 하는 함수인데?**
  * 뷰 컨트롤러가 뷰 hierarchy에 추가된 직후에 호출

* **오버라이드 가능 여부** : **O**
  * 오버라이딩 목적 : 뷰를 나타내는 것 관련해서 추가적인 task를 수행할 때  
    ex: 데이터 불러오기, 애니메이션 시작하기 등
  * view가 스크린에 보여지자 마자 실행되어야 할 코드 작성 
* **주의사항**
  * 오버라이딩 할 때, `super`를 호출해야 함



### 오버라이딩 다 되는데?

..^^; 다 되네...  
하지만!ㅋ

ViewController의 life cycle 함수를 오버라이딩 할 땐 VC 클래스 내에서,  
UIView의 life cycle 함수를 오버라이딩 할 땐 self.view.어쩌구()라던가 UIView 클래스 내에서...  
써야 된다., (너무 당연..하지만 리마인드,,)

올바른 목적으로 ~

### 다음 포스팅에서는...

`intrinsicContentSize`에 대해서 알아보고, 그 후에는 

중간중간 `setNeedsUpdateConstraints()` 같은 처음 보는 함수가 나왔는데,  
요놈들에 대해 알아보자..! (Triggering Auto Layout Functions) 뭐 하는 함수이고, 언제 쓰여야 하는 지!



### 출처

[야곰닷넷](https://yagom.net/forums/topic/loadview%EC%99%80-viewdidload-%EC%B0%A8%EC%9D%B4%EC%97%90-%EB%8C%80%ED%95%9C-%EC%A7%88%EB%AC%B8%EC%9E%85%EB%8B%88%EB%8B%A4/) [김종권의 iOS 앱 개발 알아가기](https://ios-development.tistory.com/195#recentEntries) [0urtrees](https://0urtrees.tistory.com/157)

