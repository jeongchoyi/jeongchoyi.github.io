---
layout: post
title: "Navigation Controller 초기 세팅하기" 
date: 2021-03-28
category: read 
excerpt: ""

---

# Navigation Controller 초기 세팅하기

> 어차피 앱 설치 최소 OS버전이 13 이상이라 AppDelegate 등 예외처리 해 줄 필요 없음!

### SceneDelegate.swift

```swift
    var navigationController: UINavigationController?
```

변수 선언 해 주고

```swift
    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        // Use this method to optionally configure and attach the UIWindow `window` to the provided UIWindowScene `scene`.
        // If using a storyboard, the `window` property will automatically be initialized and attached to the scene.
        // This delegate does not imply the connecting scene or session are new (see `application:configurationForConnectingSceneSession` instead).
        
        let tabbarStoryboard = UIStoryboard(name: Const.Storyboard.Name.tabbar, bundle: nil)
        let tabbarViewController = tabbarStoryboard.instantiateViewController(withIdentifier: Const.ViewController.Identifier.tabbar)
        self.navigationController = UINavigationController(rootViewController: tabbarViewController)
        self.window?.rootViewController = self.navigationController
        
        guard let _ = (scene as? UIWindowScene) else { return }
    }
```

willConnectTo 함수 내부에 rootVC 설정해주면 된다.

어차피 온보딩때문에 분기처리 해야하지만... 우선 간단히 이렇게만 해놓기!



### Extension 작성하기

네비게이션 바를 숨기거나, 뒤로가기 버튼이 있거나 없거나를 각각 VC에서 해주기는  
코드가 너무 더러워지기 때문에... Extension을 작성해서 한 곳에서 코드를 관리하기!

어차피 기본 네비 바 디자인을 쓸 게 아니기 때문에.. 디자인 때문에라도  
네비바를 사용하는 모든 VC에서 init 처리를 해 줘야 하니까~

<img src="https://user-images.githubusercontent.com/28949235/112751874-30a39900-900b-11eb-938a-771bb10dc231.png" alt="image" style="zoom:25%;" />

그냥 배경색만 지정해주면 된다.  
해당 색은 앱에서 자주 쓰이기 때문에... Color Asset에 등록하고 사용할 것!

<img src="https://user-images.githubusercontent.com/28949235/112751870-2aadb800-900b-11eb-8642-012f4a8ad129.png" alt="image" style="zoom:50%;" />

![image](https://user-images.githubusercontent.com/28949235/112751948-88da9b00-900b-11eb-97b1-81515b4ad995.png)

화이트톤 UI로 바뀐다고 하긴 했지만.. 혹시 모르니까 .. 다크모드 설정도 다 해놓고요..  
제플린에 Color Palette 설정된 게 없어서 그냥 내맘대로 네이밍 했다.

Color Asset으로 관리하면 Const 파일을 선언하기보단... UIColor의 Extension으로 const를 관리한다.

<img src="https://user-images.githubusercontent.com/28949235/112752047-00a8c580-900c-11eb-919e-14a5d967c6a2.png" alt="image" style="zoom:50%;" />

### UINavigationController+Extension.swift

VC 파일에서 navigation bar를 init해줄 땐

```swift
self.navigationController?.navigationBar.setBackgroundImage(UIImage(), for: .default)
self.navigationController?.navigationBar.shadowImage = UIImage()
self.navigationController?.navigationBar.isTranslucent = true
```

이런 식으로 `self.navigationController?.`를 붙여줬지만,  
`extensnion UINavigationController {} ` 내부에서는  
**UINavigationController의 관점에서 작성**해야 하기 때문에  
`self.navigationController?.`를 떼 줘야 코드가 작동한다.

```swift
func initializeNavigationBarWithoutBackButton() {
        navigationBar.shadowImage = UIImage()
        navigationBar.barTintColor = UIColor.Black
        navigationBar.isTranslucent = false
    }
```

이런식으로 만들어 준 후, 해당 VC의 viewDidLoad()에서 호출해주면 된다!

### Back button 이미지, 색상 변경하기

```swift
// back button 설정
navigationBar.backIndicatorImage = UIImage(systemName: "chevron.backward")
navigationBar.backIndicatorTransitionMaskImage = UIImage(systemName: "chevron.backward")
navigationItem?.backBarButtonItem = UIBarButtonItem(title: "", style: .plain, target: nil, action: nil)
navigationItem?.backBarButtonItem?.tintColor = .white
```

코드는 위와 같은데, 지금 Extension이 UINavigationController의 Extension이기 때문에  
navigationItem을 제대로 못 인식(?)한다.  
그래서 호출하는 VC에서 navigationItem을 넘겨줘야 한다.

```swift
func initializeNavigationBarWithBackButton(navigationItem: UINavigationItem?) {
```

함수 시그니쳐 부분을 이렇게 변경하고, 호출할 때

```swift
self.navigationController?.initializeNavigationBarWithBackButton(navigationItem: self.navigationItem)
```

이렇게 호출하면 된다. ( ◠‿◠ )  
주의할 점은 back button이 나타나는 VC 말고, 그 전 VC에서 호출해야 한다는 점 !

hidden도 똑같다.

```swift
    // back button이 없는 navi bar
    func initializeNavigationBarWithoutBackButton(navigationItem: UINavigationItem?) {
        navigationBar.barTintColor = UIColor.Black
        navigationBar.shadowImage = UIImage()
        navigationBar.isTranslucent = false
        
        // back button 숨기기
        navigationItem?.hidesBackButton = true
    }
```



## 추가 - back button 지정하는 함수 리팩토링

다 좋은데... 백 버튼 왼쪽 여백이 너무 좁아서 ... 구현 방법을 바꿨다.

![image](https://user-images.githubusercontent.com/28949235/112757793-35297b00-9026-11eb-9f9e-9f2dd94b8ec4.png)

원래는   

```swift
navigationBar.backIndicatorImage = UIImage(systemName: "chevron.backward")
navigationBar.backIndicatorTransitionMaskImage = UIImage(systemName: "chevron.backward")
navigationItem?.backBarButtonItem = UIBarButtonItem(title: "", style: .plain, target: nil, action: nil)
navigationItem?.backBarButtonItem?.tintColor = .black
```

이거였고, back button이 나타나기 전 뷰에서 함수를 호출해줘야 했다.

리팩토링한 함수 body 부분은

```swift
let backButton = UIBarButtonItem(image: UIImage(systemName: "chevron.backward"), style: .plain, target: self, action: #selector(touchBackButton))
backButton.tintColor = UIColor.Black

navigationItem?.leftBarButtonItem = backButton
```

backBarButtonItem을 쓰지 않고 leftBarButtonItem을 사용하는 식으로 바꿨다.

![image](https://user-images.githubusercontent.com/28949235/112757869-7883e980-9026-11eb-9440-924283f62606.png)

확실히 여백이 더 편안하게 생긴다...... ( ◠‿◠ ) 직관적인 프로퍼티 안 쓰는게 좀 찝찝하긴 하지만...  
또, **이 함수는 back button이 나타나는 뷰에서 사용한다!!**

