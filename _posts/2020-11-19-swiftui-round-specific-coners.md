---
layout: post
title: "SwiftUI 특정 모서리만 둥글게 처리하기" 
date: 2020-11-19
category: read 
excerpt: ""

---

# SwiftUI - make specific coners rounded

> SwiftUI 사각형 특정 모서리 둥글게 만들기 cornerRadius

![image](https://user-images.githubusercontent.com/28949235/99621733-f548fa80-2a6b-11eb-9d5a-bdb8864792bd.png)

> 요런 거

### /Extensions/View+Extensions.swift

파일을 만들어주고, 아래와 같이 작성

```swift
import SwiftUI

extension View {
    func cornerRadius(_ radius: CGFloat, corners: UIRectCorner) -> some View {
        clipShape( RoundedCorner(radius: radius, corners: corners) )
    }
}

struct RoundedCorner: Shape {

    var radius: CGFloat = .infinity
    var corners: UIRectCorner = .allCorners

    func path(in rect: CGRect) -> Path {
        let path = UIBezierPath(roundedRect: rect, byRoundingCorners: corners, cornerRadii: CGSize(width: radius, height: radius))
        return Path(path.cgPath)
    }
}
```



### 저 기능이 필요한 View.Swift

```swift
Rectangle()
	.cornerRadius(50, corners: .bottomRight)
	.cornerRadius(50, corners: .bottomLeft)
```

요런식으로 사용하면 된다!



[출처](https://stackoverflow.com/questions/56760335/round-specific-corners-swiftui)