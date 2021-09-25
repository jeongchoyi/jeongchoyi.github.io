---

layout: post
title: "우리 NavigationBar가 달라졌어요 (Xcode 13 update)" 
date: 2021-09-25
category: read 
excerpt: ""

---

### 우리 NavigationBar가 달라졌어요 (Xcode 13 update)

Xcode 13 업데이트 이후 잘 돌아가던  
UINavigationController, UINavigationBar, UITabbar가 다같이 난리가 났다.

![image](https://user-images.githubusercontent.com/28949235/134762301-356ea86f-1e2c-4a07-a556-66ce4f5c8b47.png)

흐릿..한 백버튼이 보이시나요...  
원래 뒤의 하얀색 뷰컨이 잘 extened 된 상태였는데,  
엑코 업데이트 이후에 갑자기 저렇게 난리가 났다.

![image](https://user-images.githubusercontent.com/28949235/134762414-3c5806a9-5fb8-4aa2-8803-c0b371b5b122.png)

이 상태라서 

```swift
self.edgesForExtendedLayout = UIRectEdge.all
```

이것도 시도해 봤는 데 안 먹고...

<img src="https://user-images.githubusercontent.com/28949235/134762359-416e267b-ac18-4d3a-95df-1caf4b2ffaa8.png" alt="image" width=400 /><img src="https://user-images.githubusercontent.com/28949235/134762346-2a02da62-9138-4a25-ab18-ebca5d488f71.png" alt="image" width=400 />

탭바도... 원래 저렇게 배경, 선이 있어야 하는데 갑자기 사라지고 투명 탭바가 됐다 ;  
whyrano... whyrano...

### 왜 이러는걸까?

결론부터 말하자면, iOS13 업데이트 때, 애플이 **UINavigationBarAppearance**라는 UIKit API를 공개했었다.   
하지만.. UINavigationBarAppearance를 쓰지 않아도 지금까지는 잘 돌아가니  
UINavigationBarAppearance로 바꿀 필요성을 못 느꼈었는데,, 이제는 써야 한다.

iOS15가 되면서 변경사항이 생겼다고 한다. Xcode 13이 iOS 15 SDK를 포함하기 때문에 위와 같은 현상이 생긴 것!  
근데,, 난 다 됐고,, 그 전(iOS14) 처럼 보였으면 좋겠는데...
-> 그럼 Appearance를 커스텀 해야 한다.

탭바는, 위 두 사진에서 오른쪽같이 생긴 게... iOS 15의 디자인이었다 (ㅋㅋㅋ)

![image](https://user-images.githubusercontent.com/28949235/134763605-a52dd463-24e7-48a0-817b-0100f93b2050.png)

iOS15의 변경사항을 살펴보려고 [WWDC 21: What's new in UIKit](https://developer.apple.com/videos/play/wwdc2021/10059/) 라는 WWDC 21 영상을 봤는데,  
에러가 아니고 ㅋㅋ 변경사항이였다. 저게 그냥 최신 디자인인 것... WWDC 챙겨볼 걸.. ( ◠‿◠ )   
스크롤 시 background가 사라지게끔 변경되었는데, 이는 내용을 더 명확하게 보여주기 위함이라고 한다.

WWDC 영상을 더 살펴보면,

![image](https://user-images.githubusercontent.com/28949235/134763741-042037a9-9f76-4cc9-9bbe-f875f744223f.png)

앞에서 공개한 새로운 모양을 적용하는 과정에서, 몇가지 문제가 발생할 수 있다고 한다.
맞다.. 바로 나한테 발생한 그 문제들 .... ㅋㅋ WWDC 진작 볼 걸!!!!!!!

bar의 isTranslucent, 반투명 속성을 false로 설정 해 둔 곳을 다시 체크해봐야 하고,  
default가 아닌 edgesForExtendedLayout을 적용한 UIViewController도 체크해봐야 한다.  
이 두가지 조건에서 visual적인 문제가 발생한다!!

![image](https://user-images.githubusercontent.com/28949235/134763865-63ce3c4f-0a10-437d-a5fc-e6ac9968068c.png)

![image](https://user-images.githubusercontent.com/28949235/134763884-eff5ae67-129f-4ab1-ac9d-13a4e296bd88.png)


### 그래서 어떻게 해결하는데?

![image](https://user-images.githubusercontent.com/28949235/134763922-7df542e9-e4e1-4689-9389-2212e3298a96.png)

답은 커스텀. 이젠 정말 .. Appearance들을 쓸 때가 온 것이다...  
custom appearance를 만들고, 그걸 bar의 `scrollEdgeAppearance` 프로퍼티에 할당해주면 된다.  
`scrollEdgeAppearance` 이 프로퍼티는 원래 UINavigationBar에만 사용 가능 했었는데,  
이제는 UIToolbar, UITabbar에도 사용가능하다.

이렇게 custom appearance를 설정하면,  
위에서 본 호환되지 않는 API들 때문에 발생하는 visual적인 이슈들은 다~ 바이바이 할 수 있다..!!!!



UIBarAppearance를 좀 더 살펴보고,  
`UINavigationController+Extension.swift` 를 갈아엎는 건 다음 포스팅에 ~~

