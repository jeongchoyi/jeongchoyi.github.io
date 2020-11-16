---
layout: post
title: "SwiftUI - placeholder" 
date: 2020-11-16
category: read 
excerpt: ""

---

# Placeholder

> redacted 라는 기능 안에 들어있다

<img src="https://user-images.githubusercontent.com/28949235/99274555-d715ab80-286d-11eb-8f3e-69913d159ff5.png" alt="image" width="300" />

```swift
.redacted(reason: .placeholder)
```

해당되는 컨텐츠 크기만큼 위 사진처럼 표시됨



### placeholder를 할지 안 할지 설정하기

```swift
@State private var showPlaceholder = false
...
//Text 의 속성으로
.redacted(reason: showPlaceholder ? .placeholder : .init())
...
Button("click"){
  showPlaceholder.toggle()
}
```

* 뭐 이런 식...
* .init() 하면 placeholder 사용하지 않는 상태



### 이미지에도 적용 가능!

적용 방법은 똑같다

 <img src="https://user-images.githubusercontent.com/28949235/99275304-abdf8c00-286e-11eb-9902-26cf3dc566b4.png" alt="image" width="300" float="left" /><img src="https://user-images.githubusercontent.com/28949235/99275335-b69a2100-286e-11eb-8558-30be8e184b94.png" alt="image" width="300" float="left" />   



**Image 동그랗게 자르고 stroke 주기**

<img src="https://user-images.githubusercontent.com/28949235/99275703-2a3c2e00-286f-11eb-9d2f-c460f77aa2d4.png" alt="image" width="300" />

```swift
Image(systemName: "person")
	.resizable()
	.frame(width:100, height:100)
	.clipShape(Circle())
	.overlay(Circle().stroke)
```

> 이건 placeholder 잡으면 overlay된 stroke 빼고 원 안의 부분만 placeholder 잡힘



### 버튼이고 뭐고 다 돼요

버튼에도 되고요... 텍스트 색도 반영되고요... 그렇다고 버튼 기능 사라지진 않음

### 이럴 때 써요

깔끔하니까 자주 씁니다 편하고 좋은기능 ~ 쓰시면 도움이 많이 된다

네트워킹이나 시간이 가는 작업을 할 때 처음엔 이렇게 가려놓고  
로드가 다 되면 보여주기 ! (페북 처음 들어갈 때 같은거인가보다...!!)

