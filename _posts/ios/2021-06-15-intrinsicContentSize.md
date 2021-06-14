---
layout: post
title: "intrinsicContentSize" 
date: 2021-06-15
category: read 
excerpt: ""

---

# intrinsicContentSize

[목차](https://iamcho2.github.io/2021/06/08/master-of-ios-life-cycle)



## 서론

VC + View Life Cycle이

![](https://user-images.githubusercontent.com/28949235/121391424-504f2a00-c989-11eb-8566-9142018c66a1.png)

이라고 굳게 믿고 있었던 나,,, `intrinsicContentSize()` 에 대해 공부하려다가 이상한 점을 발견하는데,,,  
프로젝트 만들어서 오버라이드 하려고 보니 안 되길래 뭔가 했더니 함수에서 변수로 바뀌었습니다..~ (대충 17년도 쯤)

### 원래는

```swift
override public func intrinsicContentSize() -> CGSize
{
   ...
}
```

요런식으로 CGSize를 반환하는 함수였지만,  

### 지금은

```swift
override open var intrinsicContentSize: CGSize {
    //...
    return myCGSize
}
```

요런식으로 사용하면 된다.

###  애초에 intrinsicContentSize가 뭔데?

### 출처

[SOF](https://stackoverflow.com/questions/38881151/intrinsiccontentsize-method-does-not-override-any-method-from-its-superclass)
