---
layout: post
title: "당근마켓 iOS 클론코딩 - 1. 네비게이션 바" 
date: 2021-07-28
category: read 
excerpt: ""

---

# 당근마켓 iOS 클론코딩 - 1. 네비게이션 바

[UITableView - Sticky header 야매로 구현하기](https://iamcho2.github.io/2020/11/02/uitableview-sticky-header)에 이어서 또 당근마켓을 클론코딩을 해보기로 했다.  
인턴 지원 기간은 끝났지만 그냥 하기로 했으니까 하는 클론코딩..~~ 어차피 하면 뭐든 배우게 된다

대신 이번에는 처음부터 차근차근 정리해보기로 결심...

### 클론할 내용

<img src="https://user-images.githubusercontent.com/28949235/127359317-d747a2e9-6871-4078-9ebe-5e3d503ac88a.jpeg" alt="IMG_8ED5F2CEFC4F-1" width=400 />

### 배경 색 하얗게

네비게이션 바는 보통 한 디자인으로 여러 뷰에서 사용하기 때문에,  
init 함수를 UINavigationController Extension에 만들어 뒀다.  

```swift
//  UINavigationController+Extensions.swift
extension UINavigationController {
    
    func setBackgroundColor() {
        navigationBar.barTintColor = UIColor.white
        navigationBar.isTranslucent = false
    }
}
```

그러면 각 뷰 컨트롤러에서  

```swift
//  NearbyViewController.swift
private func initNavigationBar() {
    self.navigationController?.setBackgroundColor()
}
```

요렇게만 사용해주면 돼서 간편하고, 코드의 중복도 방지할 수 있다.  
( 이번 클론에서는 배경색 외에는 지정해줄 것이 없어서 setBackgroundColor로 했으나,  
back button 디자인이 있는 경우 같이 넣어준다든가 .. back button을 없애는 작업도 할 수 있다)

![image](https://user-images.githubusercontent.com/28949235/127365230-2bb3ab94-6c61-4b3f-84d6-e0d73164efc4.png)

> 근데 고민인 것.. 이걸 UINavigationController Extensions에 넣어야 하는 지  
> UINavigationBar Extensions에 넣어야 하는 지 모르겠다.  
> 코드 쓸 때 편한 건 UINavigationController이지만, 맥락 상 UINavigationBar에 넣어야 할 것 같았다.
>
> 후에 UINavigationBar로 옮겼다.

### 네비게이션 바 타이틀 왼쪽에 배치하기

NavigationController에는 NavigationItem이라는 게 있는데, .title으로 타이틀을 지정해줄 수 있다.  
그렇게 하면 타이틀이 가운데 정렬이 되기 때문에, title 대신 item으로 넣어줘야 한다.

당근마켓 Nearby 탭의 경우 타이틀에 대한 터치 이벤트도 없고.. 그냥 Label로 넣어주면 될 것 같다.

```swift
// 타이틀 지정
let titleLabel = UILabel()
titleLabel.textColor = UIColor.black
titleLabel.text = "내 근처"
titleLabel.font = UIFont.systemFont(ofSize: 20, weight: .bold)
self.navigationItem.leftBarButtonItem = UIBarButtonItem.init(customView: titleLabel)
```

UILabel을 만들고, 색이나 굵기, 크기 등을 지정해준 후  
leftBarButtonItem으로 넣어주는데, init(customView: ) 함수를 사용해 초기화한 UIBarButtonItem을 만들어 줄 수 있다.  
(UILabel은 UIView를 상속)

이 부분도 당근마켓의 다른 탭에서 내용만 바뀔 뿐 공통적으로 사용되기에, UINavigationItem Extension에 넣어줬다.

```swift
self.navigationController?.setTitle(title: "내 근처")
```

이렇게 사용해주면 되는데, 고정된 텍스트나 수치 등을 네임스페이스 내에서 관리하기 위해 Enum을 하나 만들어줬다.

```swift
enum Nearby {
    static let title: String = "내 근처"
}
```

요렇게 만들어두면 아래와 같이 사용할 수 있다.

```swift
self.navigationItem.setTitle(title: Nearby.title)
```

![image](https://user-images.githubusercontent.com/28949235/127365752-b7facac3-8ea2-4e40-9fad-99c560774687.png)

### 오른쪽 Bar button들

처음엔 3개였다가 스크롤이 시작되면 돋보기 아이콘이 스륵 하며 올라온다.  
rightBarButtons 배열에 추가하고 빼는 식으로 하면 될 것 같다고 구상했다. 아이콘은 그냥 다 SFSymbol을 사용했다.

그냥 bar button을 만들면 UIBarButton frame의 width값이 커서 버튼들이 띄엄띄엄 있어 보이는데,  
EdgeInset을 조절하게 되면.. 디버거를 켜보면 아이콘이 한쪽으로 쏠려 있다. (찝찝)

개인적으로 UIBarButtonItem의 크기를 아예 조절하는 게 낫다고 생각해서 아래 함수를 사용한다.

```swift
//  UINavigationItem+Extensions.swift
func makeSFSymbolButton(_ target: Any?, action: Selector, symbolName: String) -> UIBarButtonItem {
    let button = UIButton(type: .system)
    button.setImage(UIImage(systemName: symbolName), for: .normal)
    button.addTarget(target, action: action, for: .touchUpInside)
    button.tintColor = .black
        
    let barButtonItem = UIBarButtonItem(customView: button)
    barButtonItem.customView?.translatesAutoresizingMaskIntoConstraints = false
    barButtonItem.customView?.heightAnchor.constraint(equalToConstant: 24).isActive = true
    barButtonItem.customView?.widthAnchor.constraint(equalToConstant: 24).isActive = true
        
    return barButtonItem
}
```

이 함수를 extension에 만들어 놓고, 

```swift
private func setRightBarButtons() {
        
    let writeButton = self.navigationItem.makeSFSymbolButton(self, 
                                                             action: Selector("pushToWrite"), 
                                                             symbolName: "highlighter")
    let scanQRButton = self.navigationItem.makeSFSymbolButton(self, 
                                                              action: Selector("pushToScanQR"), 
                                                              symbolName: "viewfinder")
    let notificationButton = self.navigationItem.makeSFSymbolButton(self, 
                                                                    action: Selector("pushToNotification"),
                                                                    symbolName: "bell")
                
    self.navigationItem.rightBarButtonItems = [notificationButton, scanQRButton, writeButton]
}
```

이런 식으로 사용하면 bar button의 크기가 딱 24*24 로 고정되어서,  
![image](https://user-images.githubusercontent.com/28949235/127371507-005a3fb7-0d02-4af8-9457-f1d4ae34b487.png)

요렇게 딱 붙어서 나온다. 만약 사이사이 spacing을 더 주고 싶으면 사이에 껴 넣어 주면 된다.



### bar button 사이 spacing 주기

![image](https://user-images.githubusercontent.com/28949235/127371774-0049d4dd-2363-459e-96db-abb80780fc50.png)

[UIBarButtonItem Apple Docs](https://developer.apple.com/documentation/uikit/uibarbuttonitem) 를 보면, fixedSpace라는 게 있다.  
넓이값을 주면 그만큼의 spacer를 만들어 주는건데, 아래와 같이 만들어 주면 된다.

![image](https://user-images.githubusercontent.com/28949235/127371977-24151fd8-7ecd-47ae-b1ba-0684f75a6b02.png)

초기화할 때 저렇게 3개의 파라미터가 있는 UIBarButtonItem을 만들어 주는데,  
`barButtonSystemItem`에 `.fixedSpace` 를 넣어주면 된다.

그리고 docs에서 나와있듯, 만든 spacer에 width값을 주면 된다.

```swift
let spacer = UIBarButtonItem(barButtonSystemItem: .fixedSpace, target: nil, action: nil)
spacer.width = 15
```

근데 또... 이렇게 코드에 숫자값을 달랑 넣긴 싫으니까 namespace에 저장해두고 사용했다.

아래는 전후비교 !

![image](https://user-images.githubusercontent.com/28949235/127371507-005a3fb7-0d02-4af8-9457-f1d4ae34b487.png)![image](https://user-images.githubusercontent.com/28949235/127372451-2859d7f5-6eac-4770-97e3-c3c6172158a8.png)



> 문득 .fixedSpace를 -20 이렇게 해서.. extension을 사용하지 않을 수도 있지 않을까? 라는 생각이 들었다.  
> 근데 Docs를 보니 음수값은 무시한다고... ( ◠‿◠ )  그래..



### 클론 완료!

클론할 것

<img src="https://user-images.githubusercontent.com/28949235/127359317-d747a2e9-6871-4078-9ebe-5e3d503ac88a.jpeg" alt="IMG_8ED5F2CEFC4F-1" width=400 />

클론한 것

![image](https://user-images.githubusercontent.com/28949235/127372451-2859d7f5-6eac-4770-97e3-c3c6172158a8.png)

