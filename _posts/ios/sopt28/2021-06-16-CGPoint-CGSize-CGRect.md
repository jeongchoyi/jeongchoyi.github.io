---
layout: post
title: "CGPoint, CGSize, CGRect" 
date: 2021-06-16
category: read 
excerpt: ""

---

## CGPoint, CGSize, CGRect

### CGPoint

> 2차원 좌표계의 점을 포함하는 구조체

```swift
public struct CGPoint {
  public var x: CGFloat
  public var y: CGFloat
  public init()
  public init(x: CGFloat, y: CGFloat)
}
```

### CGSize

> 너비와 높이값을 나타내는 구조체 (사각형이라는 뜻은 아님 !!)

``` swift
public struct CGPoint {
  public var width: CGFloat
  public var height: CGFloat
  public init()
  public init(width: CGFloat, height: CGFloat)
}
```

### CGRect

> 사각형을 표현하기 위한 구조체  
> 너비, 높이 : CGSize, 원점(origin): CGPoint

```swift
public struct CGRect {
  public var origin: CGPoint
  public var size: CGSize
  public init()
  public init(origin: CGPoint, size: CGSize)
}
```



> SOPT 28기 iOS 파트 2차 복습 자료 내용 中

