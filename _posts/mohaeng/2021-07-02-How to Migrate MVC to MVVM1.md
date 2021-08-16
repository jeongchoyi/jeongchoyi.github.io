---
layout: post
title: "How to Migrate MVC to MVVM - 1" 
date: 2021-07-02
category: read 
excerpt: ""

---

# iOS 프로젝트에 MVVM 도입하기 - 1

> MVVM 이해하기

### MVVM

**M** : Model, 네트워크 통신과 관련된 데이터 덩어리, 비즈니스 로직  
	모델이 데이터에 대한 작업을 마치면 뷰 모델에게 결과를 알림

**V** : View, 뷰의 레이아웃과 생명주기 담당 (Storyboard/Xib + ViewController)   
	사용자 이벤트를 수신하고 데이터를 표시하는 유저 인터페이스를 책임짐

**VM** : View Model, Model로부터 가져온 데이터를 View에 적합한 형태의 데이터로 가공  
	뷰 모델은 로직을 담당하고 있고, 유저가 뷰에서 어떤 액션을 취할 때 모델을 변경하거나  
	변경 되었을 때 해당 모델을 업데이트하고 뷰모델에게 결과를 알리고 뷰를 갱신함

* View Model이 변경될 때 마다 대응되는 View 화면에 자동으로 반영됨
* 1개의 View에는 최소 1개의 View Model이 존재 (View Model과 View는 1:n 관계)

**장점**

* View와 비즈니스 로직을 분리할 수 있다
* 비즈니스 로직과 database 처리를 분리 할 수 있다
* 이해하기 쉽고, 가독성이 좋다
* 생명주기 관리가 용이하다
* 테스트가 용이하다. 뷰 모델에서는 UIKit 관련된 코드가 없으므로 UI에 독립적인 테스트를 할 수 있다.

**동작 순서**

1. 사용자의 Action이 View를 통해 들어온다
2. View에 Action이 들어오면, Command 패턴으로 View Model에 Action을 전달한다
3. View Model은 Model에게 데이터를 요청한다
4. Model은 View Model에게 요청받은 데이터를 응답한다
5. View Model은 응답 받은 데이터를 가공하여 저장한다
6. View는 View Model과 Data Binding하여 화면을 나타낸다

### MVC -> MVVM

기존 MVC 패턴에서의 Controller 부분이 View와 View Model로 분리된다.  
View Model : Model로부터 데이터를 가져오는 부분,  
View : 뷰에 데이터가 뿌려지는 부분  
으로 분리된다.

이 글을 봐도 이해가 되지 않는다면 같은 주제의 다른 포스팅은 [여기](https://iamcho2.github.io/2020/11/11/swift-mvc-and-mvvm)  
(SwiftUI 글이긴 한데 개념은 동일~!!)

