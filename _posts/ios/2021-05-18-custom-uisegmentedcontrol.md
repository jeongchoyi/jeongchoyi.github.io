---
layout: post
title: "UISegmentedControl 커스텀하기" 
date: 2021-05-18
category: read 
excerpt: ""

---

# UISegmentedControl 커스텀하기

## 디자인 커스텀하기

<img src="https://user-images.githubusercontent.com/28949235/118979554-8b29f780-b9b3-11eb-857e-44bc0a558b2b.png" alt="image" style="width:500px" />

못생긴 UISegmentedControl 커스텀하기



### 배경색 투명하게 하기

```swift
let backgroundImage = UIImage()
self.mainSegmentedControl.setBackgroundImage(backgroundImage, for: .normal, barMetrics: .default)
self.mainSegmentedControl.setBackgroundImage(backgroundImage, for: .selected, barMetrics: .default)
self.mainSegmentedControl.setBackgroundImage(backgroundImage, for: .highlighted, barMetrics: .default)
```

backgroundImage를 state마다 다 UIImage()로 지정해주기
<img src="https://user-images.githubusercontent.com/28949235/118979944-f4116f80-b9b3-11eb-8948-8f79622e11ad.png" alt="image" style="width:500px" />

여기까지 하면 이상태!

### 구분선, 텍스트(선택 시)

```swift
let deviderImage = UIImage()
self.mainSegmentedControl.setDividerImage(deviderImage, forLeftSegmentState: .selected, rightSegmentState: .normal, barMetrics: .default)
self.mainSegmentedControl.setTitleTextAttributes([NSAttributedString.Key.foregroundColor: UIColor.gray], for: .normal)
self.mainSegmentedControl.setTitleTextAttributes([NSAttributedString.Key.foregroundColor: UIColor.redOrange, .font: UIFont.systemFont(ofSize: 13, weight: .semibold)], for: .selected)
```

<img src="https://user-images.githubusercontent.com/28949235/118980131-21f6b400-b9b4-11eb-8c15-1c9023b98494.png" alt="image" style="width:500px;" />

끄읕!

## 액션 핸들러 추가하기

handler

```swift
mainSegmentedControl.addTarget(self, action:#selector(mainSegmentedControlValueChanged(_:)), for: .valueChanged)
```

action

```swift
@objc func mainSegmentedControlValueChanged(_ sender: UISegmentedControl) {
    switch sender.selectedSegmentIndex {
    // 추천
    case 0:
        self.scrollToRow(row: 1)
    // 얼리버드
    case 1:
        self.scrollToRow(row: 2)
    // 기획전
    case 2:
        self.scrollToRow(row: 3)
    // HOT
    case 3:
        self.scrollToRow(row: 4)
    // NEW
    case 4:
        self.scrollToRow(row: 5)
    default:
        break
    }
}
```

값이 변할 때 마다 selectedSegmentIndex에 따라 분기처리해서 동작 수행하도록 하기!