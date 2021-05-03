---
layout: post
title: "SwiftUI - navigation view" 
date: 2020-11-16
category: read 
excerpt: ""

---

# NavigationView

> [처음 공부할 때](https://iamcho2.github.io/2020/11/03/swiftui-form-navigationview-state) 잠깐 봤지만 다시 또 공부해보자!

화면을 전환할 때 stack으로 쭉 쌓아서 depth를 표현하기도 하고,  
title을 표현하기도 함, 기능도 많고 필요한 옵션도 많다

```swift
NavigationView{
  // 이 안에 구조를 잡아야 함
}
```



## 기본 흐름

### Bar Title

```swift
NavigationView{
  List {
    Text("Hello")
    	.padding()
  }
  .navigationBarTitle("Title", displayMode: .large)
}
```

* 적당한 곳에 .navigationBarTitle
  * 그냥 Title이 아니라 Bar Title을 쓰는 이유는 displayMode때문!
* displayMode?
  * .large, .inline, .automatic(이건 나중에 설명)
  * 움직였을 때 왔다갔다 하려면 contents가 scroll이 가능해야 함. List처럼



### Navigation Link

> 보통 Navigaton Link와 연계해서 사용
>
> cell을 클릭했을 때 detail 화면으로 가야한다 라는 개념

```swift
NavigationView{
  NavigationLink("click me", destination:
              Text("detail") // 여기다 디테일 뷰 스트럭쳐 쓰면 됨
              )
  .navigationBarTitle("hello",
                     displayMode: .large)
}
```



List랑 같이 쓰려면? Text를 NavigationLink로 잡아주면 됨  
대신 String이 아니고 **(destination: label:)**이거 쓰면 됨

<img src="https://user-images.githubusercontent.com/28949235/99279211-4e9a0980-2873-11eb-9474-900e2186c69d.png" alt="image" width="300" /> 

```swift
NavigationView{
  List {
    NavigationLink(destination: 
                  Text("destination"), //화면 잡아놨으면 화면 쓰면 됨
                  label: {
                    HStack{
                      Image(systemName: "person")
                      Text("navigate")
                    }
                  })
  }
  .navigationBarTitle("Title", displayMode: .large)
}
```



## 추가 옵션들

해당되는 struct에 init()함수 구현해서 쓰면 됨 ~

```swift
init(){
  
}
```

### bar title 색상 바꾸기

```swift
UINavigationBar.appearance().titleTextAttributes = [.foregroundColor : UIColor.red]
```

* titleTextAttributes 는 key value로 되어있음 (NSAttributedString.Key)



근데 이런식으로 하다보면 설정이 많아지면 좀 애매해진다.  
그래서 실제로는 객체를 만들어서 사용하는 방법을 주로 씀

```swift
init(){
  let myAppearance = UINavigationBarAppearance()
  
  // standard 상태의 text 설정. large일때는 largeTitleTextAttributes
  myAppearance.titleTextAttributes = [
    .foregroundColor : UIColor.red,
    .font : UIFont.boldSystemFont(ofSize: 20)
    // 계속 속성 추가 가능
  ]
  
  myAppearance.backgroundColor = .orange
  
  //standard한 상태에서의 appearance 설정
  //.large의 커져있는 상태 (스크롤 전)에는 반영되지 않음. 이때는 .scrollEdgeAppearance
  UINavigationBar.appearance().standardAppearance = myAppearance
}
```

* .compactAppearance는 standard 보다 더 작아질때 쓰는데,  
  가로모드일때나 다른 기기일때 사용
* **.large**일 때
  * myAppearance.largeTitleTextAttributes = []
  * .UINavigationBar.appearance().scrollEdgeAppearance



#### 주의

객체로 바꾸는 형식을 사용하지 않고 처음 배웠던 것 처럼 바로

```swift
UINavigationBar.appearance().backgroundCOlor = .orange
```

이런 식으로 바꿔주면

<img src="https://user-images.githubusercontent.com/28949235/99280606-de8c8300-2874-11eb-8f8a-e82543aeb1e9.png" alt="image" width="300" />

이렇게 status bar 빼고 배경색이 바뀐다!  
말그대로 딱 appearance라고 잡힌 영역만 색이 바뀌는 것  
이런것들에서 객체로 바꾸는 방법이랑 좀 차이가 있다.



### detail view에서 title 색 바꾸고 싶을 때

```swift
UINavigationBar.appearance().tintColor = .black
```



### UIKit

이런것들은 UIKit쪽에 있는 설정들이라 UIKit에서 예전부터 사용하던 것들임  
더 세부적으로 설정하고 싶으면 UINavigationBarAppearance에 대해서 더 찾아보면 된다!



#### init() 내부에 해야된다는 걸 기억하자



