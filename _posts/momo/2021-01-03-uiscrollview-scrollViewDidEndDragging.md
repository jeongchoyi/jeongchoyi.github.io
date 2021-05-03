---
layout: post
title: "UIScrollView scrollViewWillEndDragging 실습" 
date: 2021-01-03
category: read 
excerpt: ""

---

# UIScrollView scrollViewWillEndDragging 실습

```swift
func scrollViewWillEndDragging(_ scrollView: UIScrollView, withVelocity velocity: CGPoint, targetContentOffset: UnsafeMutablePointer<CGPoint>) {
        <#code#>
}
```

### Parameters

```
velocity
```

The velocity of the scroll view (in points) at the moment the touch was released.
난 속력은 상관 없고 방향을 체크하는 용도로 사용!

velocity < 0 : 아래로 스크롤
velocity > 0 : 위로 스크롤

> velocity가 params로 없는 함수에서는 아래와 같이 velocity 값을 detect 할 수 있는 듯
>
> `let offsetY = scrollView.contentOffset.y`

```
targetContentOffset
```

The expected offset when the scrolling action decelerates to a stop.



오잉 근데 이 기능 사라짐 ㅎ