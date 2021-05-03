---
layout: post
title: "iOS 프로젝트 상수 관리하기" 
date: 2021-03-24
category: read 
excerpt: ""

---

# iOS 프로젝트 상수 관리하기

String이랑 Color 값들을 Asset에 잘 정리해두기!로 했는데,  
전 프로젝트를 진행할 때 했던 방법이 최선의 방법이라는 확신은 없어서  
더 공부하고 정리하기로 했다~!

### 전에 사용했던 AppConstants 파일 구조

전 프로젝트에서는 Constants들을 아래와 같이 관리했다.

```swift
//
//  AppConstants.swift
//  MoMo
//
//  Created by 초이 on 2020/12/28.
//

import UIKit

struct Constants {
    
    // MARK: - Name Contants
    
    struct Name {
        
        // MARK: - Storyboard Name Constants
        
        static let splashStoryboard: String = "Splash"
        
      // MARK: - Nib Name Constants
        
        static let homeDayNightViewXib: String = "HomeDayNightView"
    }
    
    // MARK: - Identifier Contants
    
    struct Identifier {
        // MARK: - ViewController
        
        static let splashViewController: String = "SplashViewController"
        
        // MARK: - UIView
        
        static let homeDayNightView: String = "HomeDayNightView"
        
        // MARK: - Xib Cell
        
        static let bubbleTableViewCell: String = "BubbleTableViewCell"
    }
    
    // MARK: - Design Constants
    
    struct Design {
        
        struct Color {
            /// Asset으로 Color 관리 할 거라 필요할 진 모르겠지만..
            /// 예시 : static let Blue = UIColor.rgba(red: 0, green: 122, blue: 255, alpha: 1) (Extension 함수 추가 필요)
            /// 사용 : Constants.Design.Color.Blue
            
        }
        
        struct Image {
            /// 예시 : static let IconHome = UIImage(named: "ico_category")
          static let homeIcSwipeDown = UIImage(named: "homeIcSwipeDown")
        }
        
        struct Font {
            /// 예시 : static let Body = UIFont.systemFont(ofSize: 16, weight: .regular)
            /// 사용 : label.font = Constants.Design.Font.Body
        }
        
    }
    
    // MARK: - Content Constants
    
    struct Content {
        /// 예시 : static let Category = "category"
    }
    
}

```

* `Name`과 `Identifiers`는 뷰 트랜지션 관련해서 잘 써먹었고,  
  처음엔 `Storyboard`, `xib` 등으로 분류를 나눌까 하다가  
  팀원들과 고민 끝에 저렇게 나눴는데 아주 잘 한 일 같다!
* `Design`  내에 `Color`와 `Image`, `Font`가 있는데,  
  `Color`는 Color asset으로 관리해서 예상대로 사용할 일이 없었고,  
  `Image`는 너무너무 잘 써먹었고,  
  `Font`는 다른 폰트를 사용 안 했어서 그런지 쓰지 않았다.
* 좀 고민되는 건 `Content` 인데, 사실 AppConstants.swift에 작성하기보단  
  그 내용들을 AppModel에 작성했던 것 같다. (전 프로젝트에서는)  
  그리고 그렇게 다른 파일에 분리해 두는 게 더 낫다고 생각한다.  
  Enum, Struct 다 다르기도 하고... ~.~



벗뜨,, 더 나은 방법이 있을지도 모른다는 생각에 구글링을 열심히 했다  
참고한 포스트들 [여기](https://betterprogramming.pub/structure-constants-in-ios-with-enums-5ca2135dcab0), [여기](https://medium.com/swlh/why-you-should-use-a-constants-file-in-swift-ff8c40af1b39), [여기](https://medium.com/@mikesand/constants-struct-static-lets-or-enums-b8ca9a8b7326), [여기](https://betterprogramming.pub/organizing-your-swift-global-constants-for-beginners-251579485046)

**전체를 다 Enum으로 빼는게 좋을까?** 했었지만  
Enum을 쓰는 경우 `rawvalue`를 계속 써야하는 불편함이 있어서  
MOMO에서 했던대로 필요한 항목에만 Enum을 쓰는게 좋다고 판단했다.

**한 파일에 Name, Identifiers, Image 등의 상수를 다 작성하는게 좋을까?**  
는... 정확한 답은 못 찾았지만 이번에는 분리해보려고 한다.  
진짜 사용해보면서 차이점을 발견해야지...

그렇다고 Image, Name, Identifiers struct들을 다 따로 파버리면  
사용에 있어서 헷갈릴 것 같아서 extension을 각각 따로 다른 파일에 작성하려고 한다.   

> This reduces potential merge conflicts. And by dividing them into different files, you keep the file size low and the overview high.

이런 이유도 있음 !

그리고 또, APIConstants랑 AppConstant를 파일명을 분리할 필요 없이,  
기존 APIConstants를 Const 구조체의 extension으로 빼면 되는 장점도 있다 !!! ㄷ ㄷ

### Const.swift 작성하기

![image](https://user-images.githubusercontent.com/28949235/112299092-ce7d2800-8cda-11eb-9424-6f502a3dd305.png)

> Const.swift

트리로 따지자면 root node에 있게 될 Const 구조체의 본체(!)는 `Const.swift`에 작성한다.  
우선 지금 상황에서 필요한 구조체들은 별로 없다 (개발 들어가기 전이라 ... )

적어보자면

* Name
  * Storyboard Name
  * Xib File Name
    * Xib? Nib? 그냥 Xib로 다 통일하기로 했다. [여기](https://zeddios.tistory.com/298) 참고

* Identifier
  * ViewController Identifier
  * Xib View Identifier
  * Xib Cell Identifier
* Image

정도인데... Image는 지금 제플린에서 받은게 아무것도 없으므로  
`Name`과 `Identifier`만 만들어보자!

![image](https://user-images.githubusercontent.com/28949235/112300063-ce315c80-8cdb-11eb-9d53-035551f24451.png)

>  Name.swift

![image](https://user-images.githubusercontent.com/28949235/112300518-47c94a80-8cdc-11eb-84fd-6c1eb992a3d6.png)

> Identifier.swift

> 블로그에서 본 것(Const를 Enum으로 정의함)과 다르게 struct로 만들었는데  
> extension을 여러군데에 만들어도 상관없나보다 ~.~

### 세부 constants 작성하기!..... 전에...

~~노가다의 시작~~ .. 아니 사실 나중의 더 큰 노가다를 방지하기 위한 작은 노가다  
위에도 언급했듯 `Name`이랑 `Identifier`는 Enum으로 작성하면 더 손해라고 판단해서  
`static let`으로 선언한다. ([여기](https://medium.com/@mikesand/constants-struct-static-lets-or-enums-b8ca9a8b7326) 참고, 나중에 생각 바뀔 수 있음!!!)

![image](https://user-images.githubusercontent.com/28949235/112301065-ece42300-8cdc-11eb-954b-a00bb7849e48.png)

> 전 프로젝트의 Name 구조체

Storyboard와 Xib Name을 같은 Name 구조체 안에 한번에 작성했기 때문에  
상수의 이름에 Storyboard면 `Storyboard`, Xib면 `Xib` 또는 `Cell`을 붙였다.

너무  
길다

🤔

저번에도 팀원들과 생각해봤던 내용이긴 한 것 같은데,

**a. Name 안에 Storyboard, Xib 구조체를 나눠서 작성할 경우**

* 값을 호출할 때 depth가 한번 더 깊어짐
  * `Const.Name.Storyboard.home` 같은 식으로 작성
  * 이럴거면 애초에 Storyboard, Xib를 나누고 그 안에 `Name`, `Identifier`가 있는 게 맞을듯?

**b. 기존 방식(?)대로 Name 안에 한번에 다 작성할 경우**

* 값을 호출할 때 depth가 하나 적음
  * `Const.Name.homeStoryboard` 같은 식으로 작성



그림으로 정리해서 고민해보자 .. ( ◠‿◠ )  아우 머리아파

![asd](https://user-images.githubusercontent.com/28949235/112303157-52391380-8cdf-11eb-97c3-d54bdf535a6a.png)

>  왜 2번이 끌리는 것일까?.. . . . ......  
>
> 아 그리고 Storyboard는 Identifier가 필요 없지 ...! 오타다

**1번의 장점**

* Name, Identifier가 더 상위 노드(분류)에 있을 시 
  * Storyboard, VC, xib가 추가돼도 파일이 추가될 필요가 없음
    * 근데 이거 `And by dividing them into different files, you keep the file size low and the overview high.` 에 반대되는 거 아닌가?
  * 2번의 경우 공통으로 사용하는  Name, Identifier 상수 구조체들이 각 항목에 따라 분리되게 되는데  
    **공통 속성들을 한 구조체로 합치기 vs 파일 분리하고 용량 낮추기**  
    결국 이건가.....?
    * 아 근데 1번에서도 Xib는 분리되긴 한다...
* `Const.Name.Storyboard.onboarding`  
  `Const.Identifier.Xib.dayNightView`  
  `Const.Name.Xib.bubbleTableViewCell`  
  `Const.Identifier.ViewController.onboarding`

**2번의 장점**

* 더 세분화된 상수 분류
* `Const.Storyboard.Name.onboarding`  
  `Const.Storyboard.Identifier.onboarding`  
  `Const.Xib.Name.bubbleTableViewCell`  
  `Const.ViewController.Identifier.onboarding`
* 더 문장같다... 문장같이 읽히는 코드가 클린코드랬는데



상수를 실제 사용할때를 생각해보자.

```swift
let splashStoryboard = UIStoryboard(name: 여기는 name값이 들어갈 것, bundle: nil)
        let splashViewController = splashStoryboard.instantiateViewController(withIdentifier: 여기는 identifier)
```

**1번 코드**

```swift
let splashStoryboard = UIStoryboard(name: Const.Name.Storyboard.splash, bundle: nil)
        let splashViewController = splashStoryboard.instantiateViewController(withIdentifier: Const.Identifier.ViewController.splash)
```

**2번 코드**

```swift
let splashStoryboard = UIStoryboard(name: Const.Storyboard.Name.splash, bundle: nil)
        let splashViewController = splashStoryboard.instantiateViewController(withIdentifier: Const.ViewController.Identifier.splash)
```

확실히 시선의 처리 방면에서 2번이 편하다 !!!

![Untitled-2](https://user-images.githubusercontent.com/28949235/112304832-4e0df580-8ce1-11eb-9fe9-f76cc8b10d22.png)

작성방향과 코드 구조가 같아서... 아니 이게 뭐라고 몇시간 동안 이렇게까지 고민하나 싶지만  
이번 이후로 최대한 다시는 같은 고민을 하지 않기 위해...!!  

![image](https://user-images.githubusercontent.com/28949235/112306604-55360300-8ce3-11eb-917b-4283671fbfac.png)

땅땅땅 👩‍⚖️

### 진짜로 세부내용 작성하기

자자 그러면 아까 만들어둔 Name, Identifier swift 파일은 다 지우고 새로 만들시간!

![image](https://user-images.githubusercontent.com/28949235/112306337-0d16e080-8ce3-11eb-8eec-aec1a3571652.png)

> Xib.Swift

다른 것들도 똑같이 만들어준다.  
Storyboard의 경우 Name만 필요하고,  
VC의 경우 Identifier만 필요하기 때문에 하위 struct들은 필요한것만 작성해주면 된다!

![image](https://user-images.githubusercontent.com/28949235/112305807-777b5100-8ce2-11eb-9331-43d0c7e40ef1.png)

우하하

이제 만들어둔 Storyboard들과 VC들을 보면서 내용을 채워넣자!  
오타 없게 조심조심조심조심!!!!

![image](https://user-images.githubusercontent.com/28949235/112307125-e5744800-8ce3-11eb-9403-c82e08b0eb41.png)

> Storyboard.swift

#오타지적환영

![image](https://user-images.githubusercontent.com/28949235/112307259-12c0f600-8ce4-11eb-966a-8244e5754964.png)

> Identifier.swift  
> (tabbar는 뺐다)

xib file은 아직 없으므로... 끝!

