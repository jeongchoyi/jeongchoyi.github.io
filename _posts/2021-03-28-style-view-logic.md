---
layout: post
title: "Style 뷰 로직 작성하기" 
date: 2021-03-28
category: read 
excerpt: ""

---

# Style 뷰 로직 작성하기

### 뷰 전환

```swift
private func pushToScentViewController() {
        let scentStoryboard = UIStoryboard(name: Const.Storyboard.Name.scent, bundle: nil)
        guard let scentViewController = scentStoryboard.instantiateViewController(withIdentifier: Const.ViewController.Identifier.scent) as? ScentViewController else {
            return
        }
        self.navigationController?.pushViewController(scentViewController, animated: true)
    }
```



### Enum으로 뷰 재사용시 usage 관리

Style 뷰는 온보딩에서는 back button이 없고,  
main 뷰를 통해 보일 때에는 back button이 있는 상태이기 때문에 분기처리가 필요하다.

```swift
    // MARK: - Properties

    enum StyleViewUsage: Int {
        case onboarding = 0, main
    }
    
    var styleViewUsage: StyleViewUsage?
```

이렇게 enum을 만들어주고, 

```swift
    // MARK: - Functions
    
    private func initializeNavigationBar() {
        
        switch self.styleViewUsage {
        case .onboarding:
            	// 온보딩일 때 처리
        case .main:
            	// 메인 플로우일 때 처리
        default:
            return
        }
    }
```

이런식으로 분기처리 해 주면 된다.  
그리고 나중에 뷰 트랜지션 처리 할 때

```swift
styleViewUsage = .main
```

위와 같이 지정해주면 된다. (온보딩 진행 시에는 .onboarding으로)

