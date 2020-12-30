---
layout: post
title: "Onboarding screen iOS 13 이상에서 적용" 
date: 2020-12-29
category: read 
excerpt: ""

---

# Onboarding screen iOS 13 이상에서 적용

엑코 버전 11에서는 프로젝트를 만들면 AppDelegate 뿐만 아니라  
SceneDelegate 파일도 생성된다.

iOS 13 이상에서는, `UserDefaults.standard.bool(forKey:"어쩌구") `사용해서  
유저가 앱을 처음 Launch 하는지 아닌지에 따라 Onboarding 스크린을 띄우는 작업을  
**SceneDelegate**에서 해야 한다. 13 이상에서 UI Lifecycle은 SceneDelegate에서 !

당연히 iOS 13 미만에서는 그대로 AppDelegate에 따라 동작하기 때문에,  
해당 OS 버전 기기를 고려하기 위해서는 AppDelegate도 동일 코드가 존재해야 한다.



근데 알아서 OS따라 맞는거 찾아서 실행하겠지~하고  
다짜고짜 AppDelegate랑 SceneDelegate에 같은 코드 때려넣으면 안 됨.  
iOS 13 이상에서도 Process LifeCycle 등 다른 목적으로 AppDelegate를 쓰긴 쓰기 때문에...

그리고 게다가 실행 순서가  
AppDelegate → SceneDelegate!

즉, AppDelegate가 먼저 실행되고 그 다음 SceneDelegate가 실행됨  
그래서 AppDelegate랑 SceneDelegate에 같은 코드를 복붙해놓으면   
AppDelegate에 있는 코드가 먼저 실행돼서 Onboarding Screen이 안 뜨고  
바로 main으로 넘어가게 된다.

이미 AppDelegate의 코드 때문에 UserDefaults의 값은 변경되어서,  
SceneDelegate의 분기문 안으로는 들어가지 않기 때문에 마찬가리고 Onboarding Screen은 볼 수 없다.

그래서 AppDelegate에는 OS 버전에 따른 분기를 추가 해 줘야 한다.  

```swift
// AppDelegate.swift - didFinishLaunchingWithOptions 함수

if #available(iOS 13, *) {
print("SceneDelegate에서 처리")

) } else { 

        let launchedBefore = UserDefaults.standard.bool(forKey: "hasLaunched")
        let launchStoryboard = UIStoryboard(name: "Onboarding", bundle: nil)
        let mainStoryboard = UIStoryboard(name: "Main", bundle: nil)
        var vc: UIViewController
        if launchedBefore
        {
            vc = mainStoryboard.instantiateInitialViewController()!
        }
        else
        {
            vc = launchStoryboard.instantiateViewController(identifier: "OnboardingViewController")
        }
        UserDefaults.standard.set(true, forKey: "hasLaunched")
        self.window?.rootViewController = vc
        
        guard let _ = (scene as? UIWindowScene) else { return }

}
```

요런 식으로!

```swift
// SceneDelegate.swift - willConnectTo 함수

    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        // Use this method to optionally configure and attach the UIWindow `window` to the provided UIWindowScene `scene`.
        // If using a storyboard, the `window` property will automatically be initialized and attached to the scene.
        // This delegate does not imply the connecting scene or session are new (see `application:configurationForConnectingSceneSession` instead).
        
        let launchedBefore = UserDefaults.standard.bool(forKey: "hasLaunched")
        let launchStoryboard = UIStoryboard(name: "Onboarding", bundle: nil)
        let mainStoryboard = UIStoryboard(name: "Main", bundle: nil)
        var vc: UIViewController
        if launchedBefore
        {
            vc = mainStoryboard.instantiateInitialViewController()!
        }
        else
        {
            vc = launchStoryboard.instantiateViewController(identifier: "OnboardingViewController")
        }
        UserDefaults.standard.set(true, forKey: "hasLaunched")
        self.window?.rootViewController = vc
        
        guard let _ = (scene as? UIWindowScene) else { return }

    }
```

