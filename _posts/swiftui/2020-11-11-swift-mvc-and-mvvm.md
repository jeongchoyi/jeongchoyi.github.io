---
layout: post
title: "MVC와 SwiftUI MVVM" 
date: 2020-11-11
category: read 
excerpt: ""

---

# SwiftUI - MVVM 디자인 패턴 개념

솝텀 프로젝트를 SwiftUI를 사용해서 진행하면서, MVVM 디자인 패턴을 적용하기로 했다!  
새로운 정보들이 막 쏟아지는 정보의 바다에서 허버허버 주워먹는 중...  
오늘 주워먹을 것은 MVVM이다~ hubba hubba ~

최종 목표는 [승욱님의 SwiftUI MVVM](https://github.com/seungwook-jung/SimpleMVVM) 완.벽.이.해 하기

### MVC, MVVM 

**MVC** - Model, View, Controller

**MVVM** - Model, View, **View Model**

MVC 디자인 패턴으로 앱을 구현하다보면,  
앱이 기능이 많고 커질수록 ViewController 파일이 엄청나게 길어진다.  
모든 비즈니스 로직이 ViewController에 추가되었다고 말을 하는데,  
암튼 이러한 문제를 피하기 위해 도입된 새로운 디자인 패턴이 MVVM이다.

> 2019년 SwiftUI를 발표하면서 애플이 MVVM 패턴을 소개함

![image](https://user-images.githubusercontent.com/28949235/98830114-5d507d00-247d-11eb-86f9-5623e5b719d8.png)

![image](https://user-images.githubusercontent.com/28949235/98833357-414eda80-2481-11eb-95dd-cbc200f3b120.png)

![image](https://user-images.githubusercontent.com/28949235/98833373-44e26180-2481-11eb-9605-0918f99ca3ca.png)

MVVM은 View(GUI)의 개발을 비즈니스 로직(or 백엔드 로직)인 Model로부터 분리한다.  
그러면 뷰가 어느 특정한 모델에 종속되지 않을 수 있다.  
ViewModel이 Model에 있는 데이터 객체를 노출, 변환하기 때문에 뷰 모델은 값 변환기 역할을 한다.

ViewModel은 View 자체의 디스플레이 로직을 제외한 대부분을 처리한다.  
View와 Model의 중간 역할을 한다.  
View-ViewModel 사이 Binding이 있는데,  
ViewModel은 Model에 변화를 주고, ViewModel을 업데이트 한다.  
그 때 Binding으로 인해 View도 업데이트 된다.

여기서 중요한 건 ViewModel은 View에 대해 전혀 알지 못한다.  
이래서 테스트가 쉬워지고, Binding으로 인해 코드 양이 많이 줄게 된다.  
그리고 ViewModel에는 Model에 대한 참조가 있다.



### Model - 데이터

데이터 모델, 데이터 접근 레이어, 비즈니스 로직  
Model이 데이터에 대한 작업 (View Model로부터 시작된 CRUD(?))을 마치면 뷰 모델에게 결과를 알려줌  

Model은 View Model이 소유하고 있고,  View나 View Model이 Model을 들여다 볼 수 없음

### View - 유저 인터페이스 UI

UI를 반영하는 코드  
생명주기 관리 코드  
ViewController  

모델을 보여주고 사용자-View의 상호작용 (클릭, 키보드 입력 등...)을 수신

수신되는 사용자 이벤트를 View Model에 전달하여 처리되도록 함 (data binding - 속성, callback 함수 등)  
View는 View Model의 변경사항을 감지하고 View Model이 업데이트한 데이터를 보여줌  
View - Model 사이엔 binding이 없고, View Model에 의해 연결됨  

> SwiftUI에서는 `@ObservedObject` 라는 property wrapper를 통해 View-View Model간 binding 수행  
> = View와 View Model은 `@ObservedObject` 를 사용해서 직접 연결됨

### ViewModel - View와 Model을 연결

Model로 부터 가져온 데이터를 View에 맞게 가공이나 처리하는 코드  
로직 담당  
View 에 대해 전혀 알지 못 함 - binding 된 채로 update를 주고 받음  

하나의 View에는 하나의 View Model이 1:1로 매칭되어있음  
ViewModel이 변경될 때 마다 binding된 View에 자동으로 반영됨

유저가 View에서 어떤 액션을 취하거나, Model을 변경하면 해당 Model을 업데이트 - View Model 업데이트 - View 갱신

> SwiftUI에서는 `ObservableObject` 라는 프로토콜을 준수해서 통신을 자동화 함  
>
> 또한 View는 `@ObservedObject` 로 연결된 View Model과 update를 주고 받음  

