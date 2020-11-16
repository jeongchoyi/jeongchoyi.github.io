---
layout: post
title: "SwiftUI - Grid" 
date: 2020-11-16
category: read 
excerpt: ""

---

# Grid

> UIKit의 CollectionView랑 비슷, 바둑판 모양으로 나열

### 어떻게 만드나요?

LazyHGrid, LazyVGrid 라는 게 있다.

<img src="https://user-images.githubusercontent.com/28949235/99267775-b77b8480-2867-11eb-8b64-8a36f8ab44fa.png" alt="image" width=400 />

``` swift
LazyVGrid(columns:
         [GridItem(.fixed(20))], content: {
           
           Image(systemName: "music.mic")
           Image(systemName: "music.mic")
           Image(systemName: "music.mic")
           Image(systemName: "music.mic")
           Image(systemName: "music.mic")
           
         })
```

* columns: 하나하나 column에 대한 크기에 대한 부분 정의
  * GridItem이라는 type의 배열 **[GridItem]**
* 텍스트로 하면 쪼금 와닿지 않는 부분이 있어서 이미지로 예시를 듦



나눠서 봅시다 (보통 이렇게 따로 빼서 작업함)

```swift
var columns: [GridItem] {
  [GridItem(.fixed(100))] //fixed: 그 값 그대로 표현 (width값)
}

...

LazyVGrid(columns: columns, content: {
           
           Image(systemName: "music.mic")
           	.resizable() //해줘야 크기가 100으로 변경됨
  					.aspectRatio(contentMode: .fit) //넓이만 100인걸 아이콘 고유의 비율에 맞게
           Image(systemName: "music.mic")
           	.resizable()
            .aspectRatio(contentMode: .fit)	
           Image(systemName: "music.mic")
           	.resizable()
            .aspectRatio(contentMode: .fit)	
           Image(systemName: "music.mic")
           	.resizable()
            .aspectRatio(contentMode: .fit)	
           Image(systemName: "music.mic")
           	.resizable()
            .aspectRatio(contentMode: .fit)	
           
         })
```

### column이 두개

> colums: [GridItem]의 배열의 element를 2개로 만들면 됨

```swift
var columns: [GridItem] {
  [
    GridItem(.fixed(50)),
  	GridItem(.fixed(50))
  ]
}
```

나열은 원소가 5개라면  
1  2  
3  4  
5  
이런 식으로 나열됨!



### GridItem에서 .fixed

.fixed란 말 그대로 그 값 그대로 고정하는 것  
--> 화면보다 더 커질 수 있고, 화면보다 더 커지면 다른 element들을 밀어낼수도 있음  
     자신도 화면에서 짤릴 수 있음

그래서 fixed로 작업할 땐 화면에서 잘리지 않게끔 그걸 염두에 두고 작업해야 할 필요가 있다.

다른 옵션도 함 보자~!

### GridItem에서 .flexible

```swift
var columns: [GridItem] {
  [
    GridItem(.flexible(minimum: 50, maximum: 200)),
    GridItem(.flexible(minimum: 50, maximum: 200))
  ]
}
```

*  column 수나 화면 크기라던가.. 암튼 그 안에서 최대한 크게 설정되는데   
  그거의 min, max값을 정해줌

### GridItem에서 .adaptive

<img src="/Users/choyi/Library/Application Support/typora-user-images/image-20201117003310466.png" alt="image-20201117003310466" width="400" />

```swift
var columns: [GridItem] {
  [
    GridItem(.adaptive(minimum: 40, maximum: 100))
  ]
}
```

* 쭈우욱 나열될 수 있는 크기만큼 나열된 후 개행
* 추천검색어나 tag box들이 쭉 나열되는 거, hashtag들이 쭉 나열되는 거.. 같은 거에 사용됨

* 어떻게 보면 flexible이랑 반대되는 개념
  * flexible은 가능한 한 크게 (maximum) 맞추는 반면에,
  * adaptive는 가능한 한 작게 (minimum). minimum으로 맞출 수 있는 상황이면 minimum으로 맞춤



### Grid는 기본적으로 스크롤이 안 된다!

그래서 ScrollView로 감싸서 표현한다~!

```swift
ScrollView(.vertical){
  LazyVGrid(columns:columns, content: {
           
           Image(systemName: "music.mic")
           Image(systemName: "music.mic")
           Image(systemName: "music.mic")
           Image(systemName: "music.mic")
           Image(systemName: "music.mic")
           
         })
}
```

LazyHGrid도 마찬가지..

```swift
ScrollView(.horizontal){
  LazyHGrid(rows:columns content: { //columns라써있는건 걍 위에 변수명이니까..
           
           Image(systemName: "music.mic")
           Image(systemName: "music.mic")
           Image(systemName: "music.mic")
           Image(systemName: "music.mic")
           Image(systemName: "music.mic")
           
         })
}
```



### 더 알아보기

이건 강좌 필기는 아니지만..  
갠적으로 WWDC 2020에 나온 세션 보고싶다

[Stacks, Grids, and Outlines in SwiftUI](https://developer.apple.com/videos/play/wwdc2020/10031)

