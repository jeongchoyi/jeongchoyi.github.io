---
layout: post
title: "TabBar에서 선택된 Tab에 따라 분기처리 하기" 
date: 2021-04-24
category: read 
excerpt: ""

---

# TabBar에서 선택된 Tab에 따라 분기처리 하기

<img src="https://user-images.githubusercontent.com/28949235/116039510-c895b080-a6a5-11eb-9d23-eeb8a871ccc9.png" alt="simulator_screenshot_6513BB5D-9508-4039-B836-E49FF2C8A9D5" style="zoom:15%;" /><img src="https://user-images.githubusercontent.com/28949235/116039524-cc293780-a6a5-11eb-85dd-9271a65e1ccc.png" alt="simulator_screenshot_1FF0406D-3A82-4F94-878A-B662EC3BD3A5" style="zoom:15%;" /><img src="https://user-images.githubusercontent.com/28949235/116039531-cfbcbe80-a6a5-11eb-8d54-6d1ddc3ddbac.png" alt="simulator_screenshot_3FCD9A8F-B815-4EA8-B838-BA7C28895F92" style="zoom:15%;" />

이렇게 탭 별로 navigation bar button을 바꿔줘야 해서  
현재 active 상태인 tab을 가져와야 했다.

<img src="https://user-images.githubusercontent.com/28949235/116039759-17dbe100-a6a6-11eb-8584-b6581e88741d.png" alt="image" style="zoom:40%;" />

TabBarController에서!  
**UITabbarControlelrDelegate**의 `didSelect`함수를 사용하면 선택된 tab을 구분할 수 있다.

```swift
// MARK: - UITabBarControllerDelegate

extension TabbarViewController: UITabBarControllerDelegate {
    override func tabBar(_ tabBar: UITabBar, didSelect item: UITabBarItem) {
        switch item.title! {
        case "메인":
            initFilterButton() // 메인 탭에서만 필요한 filter button 추가
        default:
            self.navigationItem.leftBarButtonItem = nil
        }
    }
    
}
```

tab bar item에 tag를 부여했다면`item.tag`로도 분기처리를 할 수 있다!