---
layout: post
title: "isTranslucent troubleshooting (iOS15, Xcode13)" 
date: 2021-09-26
category: read 
excerpt: ""
---

# isTranslucent troubleshooting (iOS15, Xcode13)

[전 글](https://iamcho2.github.io/2021/09/25/UINavigationController-UINavigationBar-Xcode13-update)에서 Xcode 업데이트 후 네비바가 아작난 사태의 범인이  
`isTranslucent`와 `edgesForExtendedLayout` 임을 알아냈고...  
이제는 고칠 시간!

![image](https://user-images.githubusercontent.com/28949235/134792428-8dbb53c7-696d-4ea3-9569-09c3813fe976.png)

먼저, 원래 문제가 됐던 (아니 이제 문제가 된) 코드 부터 보자면,,,
barTintColor, shadowImage 같은 건 상관 없고, `가 문제였다.

필요 없는 코드를 지워보자

![image](https://user-images.githubusercontent.com/28949235/134792449-3d0e9334-f22e-4da5-8be0-ea2732f158c3.png)

요 상태에서 빌드하면,

<img src="https://user-images.githubusercontent.com/28949235/134792462-bbc0b8bd-234c-4076-8ecd-c8f3b57dd108.png" alt="image" width=400 />

아작난다. view hierarchy는 아래와 같다.

![image](https://user-images.githubusercontent.com/28949235/134792478-f57e60be-7bb6-486d-a3ed-b667b7d38455.png)

NicknameViewController의 상단이  
UINavigationController의 top만큼 extended 되어있지 않다.

![image](https://user-images.githubusercontent.com/28949235/134792498-6ee853f8-d742-4041-b4ea-4fcbb1383c9e.png)

그러니까 당연히 navi bar의 배경이 없으니까, 검정색 navi bar가 되는 것!  
그럼 이 상태에서, `isTranslucent`를 true로 바꿔보자.

![image](https://user-images.githubusercontent.com/28949235/134792538-93de2e38-aadf-4623-9818-12d8c5455695.png)

ㄸㅣ용.. `isTranslucent`가 VC의 extend를 막는거여..?!?!?!  
내가 `edgesForExtendedLayout = UIRectEdge.all` 했을 땐 먹지도 않드만,,,  
그리고 투명이네??? 그럼 난 .. 그동안 왜 `isTranslucent`를 굳이 명시적으로 false로 사용했던거드라...?

![image](https://user-images.githubusercontent.com/28949235/134792660-a65afdc6-6cda-4c0f-8089-f158617fe3f5.png)

분명히 예전엔 `isTranslucent`를 명시적으로 지정해주지 않으면 안 됐던 것 같아서,, 
시뮬도 다운받았다 ㄱ-... 이미 내 폰도 iOS15로 올려버렸기 때문에,,

### iOS 15 이전 버전에서 isTranslucent

iOS14 버전으로 빌드를 해보면,

![image](https://user-images.githubusercontent.com/28949235/134793470-b52cf177-45dd-4956-98a4-50783580241f.png)

(`isTranslucent = true` 일 때)

![image](https://user-images.githubusercontent.com/28949235/134793502-eced58e3-6bda-4cce-bda0-f8d242f94918.png)

(`isTranslucent = false` 일 때)

차이점이 있다!

* `isTranslucent = true` 일 때
  * topmost VC가 navigationController의 최상단까지 확장된다.
  * 색상이 지정한 색 대로 나오지 않고, 약~간 반투명 처리가 된다.
* `isTranslucent = false` 일 때
  * topmost VC가 확장되지 않는다. (navibar의 끝 == VC의 시작)
  * 색상이 지정한 색 대로 나온다.

iOS 15로 업데이트 되기 전, 보통 투명한 Navi bar를 사용했었는데,  
이를 위해 `isTranslucent`를 true로 지정해놓고 사용했던 것!

```swift
navigationBar.setBackgroundImage(UIImage(), for: .default)
navigationBar.shadowImage = UIImage()
navigationBar.isTranslucent = true
```

![image](https://user-images.githubusercontent.com/28949235/134793616-9078cf80-1494-48c1-a8a5-e97b7070d90d.png)

여기서 `isTranslucent`만 false로 바꾸면,  

```swift
navigationBar.setBackgroundImage(UIImage(), for: .default)
navigationBar.shadowImage = UIImage()
navigationBar.isTranslucent = false
```

![image](https://user-images.githubusercontent.com/28949235/134793639-4e263eaa-76e0-4866-a322-8ba742ffeda9.png)

이렇게 navigation bar 에 UIBarBackground 가 생긴다 (ImageView)

![image](https://user-images.githubusercontent.com/28949235/134793666-30da75da-210c-44fa-ac41-15c1fdff9788.png)

그래서 만약 내 뷰의 배경이 SystemBackgroundColor가 아니라 다른 색이라면,

![image](https://user-images.githubusercontent.com/28949235/134793676-d23cb64a-063e-4f3f-93ff-6b12b484e403.png)

이런 사태가 발생해서 결국

<img src="https://user-images.githubusercontent.com/28949235/134793683-b392e698-e778-47db-ac2e-c8f1b42c41fe.png" alt="image" width=400 />

이런 navigation bar가 되는 것이다. (투명이 안 됨)



정리하자면!! 투명 navigation bar를 사용하기 위해 `isTranslucent`를 true로 설정해 놨었는데,,  
이번에 iOS 15로 넘어오면서 visual issue가 발생하게 된 것이다.

### 다시 iOS 15로 넘어와서

그럼, 고치려면 `isTranslucent`만 true로 바꾸면 되는 거 아니야? false가 문제라며?

![image](https://user-images.githubusercontent.com/28949235/134793827-6c03addf-000f-44d0-ae2c-4565d6425149.png)

![image](https://user-images.githubusercontent.com/28949235/134793848-fc35462c-9295-4cff-88d5-84b5b34ba499.png)

뭐 .. false였던 게 문제였으니.. true로 바꿔주면 iOS15에서는 잘 돌아간다.  
![image](https://user-images.githubusercontent.com/28949235/134793868-d57afdb9-503c-4cc4-afb5-0eb129e35c5c.png)

iOS 14에서도 잘 나온다.

### isTranslucent를 true로만 하면 문제가 없는걸까?

```swift
navigationBar.barTintColor = .red
```

만약, navigation bar가 투명이 아니라 배경색이 있어야 한다면?

![image](https://user-images.githubusercontent.com/28949235/134793976-773d131a-f87b-4acb-8ef6-4add756cf433.png)

![image](https://user-images.githubusercontent.com/28949235/134793983-fa7eac2b-e67c-486b-977c-d57a0d64a37a.png)

(iOS14)

![image](https://user-images.githubusercontent.com/28949235/134793995-ac29f96c-0747-4d4e-a26c-e268631418ef.png)

(iOS15)

안먹는다. **iOS14에서는, barTintColor는 isTranslucent가 false여야 먹는다.**

![image](https://user-images.githubusercontent.com/28949235/134794002-c5e2fbb1-3f76-4a3c-a735-f5858dba7e70.png)

![image](https://user-images.githubusercontent.com/28949235/134794024-9a6db595-a35b-4768-9206-0bfa0c71e435.png)

(iOS14)

근데... false 안된다고 했자나.. iOS15에서 visual issue 있다고 했자나...

![image](https://user-images.githubusercontent.com/28949235/134794034-29b4eaec-d418-44ba-9091-01a893d08167.png)

(iOS15)



## UIBarAppearance

그래서 이젠 피할 수 없는 UIBarAppearance...

```swift
let appearance = UINavigationBarAppearance()
appearance.backgroundColor = .red
self.navigationBar.standardAppearance = appearance
self.navigationBar.scrollEdgeAppearance = appearance
```

(scrollEdgeAppearance는 iOS15를 위해)  
이렇게 설정해주면

![image](https://user-images.githubusercontent.com/28949235/134794076-0aa56dc4-1e9b-4d6a-a1db-fadb8acd507c.png)

(iOS15)  
iOS 15에서 잘 나온다. iOS 14에서는?

![image](https://user-images.githubusercontent.com/28949235/134794116-7bd821d6-1d8a-41de-bb10-dddd533ddfc0.png)

#나한테왜이러는데...

<img src="https://user-images.githubusercontent.com/28949235/134794318-dd9ab1aa-a91a-44e5-8f90-4ccf0e620b9e.png" alt="image" width=600 />

Apple에서 배포하는 [Customizing Your App’s Navigation Bar](https://developer.apple.com/documentation/uikit/uinavigationcontroller/customizing_your_app_s_navigation_bar) 샘플 프로젝트에서도 똑같은 현상이 발생했다.  
iOS14에서 

```swift
appearance.backgroundColor = .red
```

이게 안 먹는다. ㄱ-

![image](https://user-images.githubusercontent.com/28949235/134794442-10955af4-2f82-48e0-9da2-4603a0fe5c05.png)

![image](https://user-images.githubusercontent.com/28949235/134794533-c592360f-cb1c-4e3d-b32f-a3f0c808d2b8.png)

저렇게

```swift
self.navigationBar.barTintColor = appearance.backgroundColor
```

이걸 추가해줘야 (꼭 appearance.backgroundColor가 아니라 .red로 해도 되겠지만) iOS14에서도 잘 돌아간다  
아 진짜 뭔데.. ㄱ- 찝찝하게시리



### 투명 Navigation Bar

우선.. 됐고 지금 나한테 필요한 건 투명 네비바다.

```swift
let appearance = UINavigationBarAppearance()
self.navigationBar.standardAppearance = appearance
self.navigationBar.scrollEdgeAppearance = appearance
```

appearance의 backgroundColor, navigationBar의 barTintColor line을 지워줬다.

![image](https://user-images.githubusercontent.com/28949235/134796578-e3d5f8fe-8444-4a8c-becc-0ad61e4fcd4a.png)

(iOS14) 잘 나오고,,

![image](https://user-images.githubusercontent.com/28949235/134796600-0f620964-3ec1-45d0-aac0-8adf9dc56fe0.png)

(iOS15) ㅋㅋ.. 아우 저 반투명 배경 좀 없애라 !!!!!!!!!!!

```swift
let appearance = UINavigationBarAppearance()
appearance.configureWithOpaqueBackground()
appearance.backgroundColor = .clear
appearance.shadowColor = .clear
self.navigationBar.standardAppearance = appearance
self.navigationBar.scrollEdgeAppearance = appearance
```

`configureWithOpaqueBackground()`로 불투명하게 만들어주고 배경색으로 clear를 줬는데,  
Docs를 살펴보던 중 더 간단한 걸 발견했다.

![image](https://user-images.githubusercontent.com/28949235/134796766-b5504d6e-f0f6-49a9-9d41-51ab8b79d7d7.png)

...!!!!!! 대박 당장써 shadow도 없애준다.

```swift
let appearance = UINavigationBarAppearance()
appearance.configureWithTransparentBackground()
self.navigationBar.standardAppearance = appearance
self.navigationBar.scrollEdgeAppearance = appearance
```

![image](https://user-images.githubusercontent.com/28949235/134796820-54c19d32-5e07-44c9-9708-564a29b2e554.png)

(iOS14)

![image](https://user-images.githubusercontent.com/28949235/134796791-74c8875a-5e46-4833-b451-c61964e999ed.png)

(iOS15)

ㄲㅑ악........... 너무 깔끔해.... 행복.....

이제 `configureWithTransparentBackground()`이 어떻게 이루어져 있는지가 궁금해졌다.  ㅋㅋㅋ  
함수 내부 내용은 볼 수 없지만,,,

```swift
appearance.configureWithOpaqueBackground()
appearance.backgroundColor = .clear
appearance.shadowColor = .clear
```

이거겠지?





