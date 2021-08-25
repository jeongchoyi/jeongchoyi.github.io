---
layout: post
title: "Swift 알고리즘 문제들을 풀면서 자주 찾게되는 것들" 
date: 2021-08-18
category: read 
excerpt: ""


---

# Swift 알고리즘 문제들을 풀면서 자주 찾게되는 것들

> 자주 찾아보게 되는 것들 정리

- **어떤 Int값이 해당 범위 내에 있는지 없는지 확인할 수 있는 방법**

```swift
if (10...100).contains(50) {
    print("Number is inside the range")
}
```

* **2진수 -> 10진수, 10진수 -> 2진수**

> 주의할 점은 10진수는 타입이 `Int`, 다른 진수의 타입은 `String` 이라는 것  
> radix를 8로 지정하면 8진수, 16으로 지정하면 16진수 됨.  
> (2와 16진수 사이 변환은 중간에 10진수를 거쳐줘야 함)

```swift
let num = 22
let str = String(num, radix: 2) // 10진수 -> 2진수
print(str) // prints "10110"
```

```swift
let binary: String = "11011"
let decimal: Int = Int(binary, radix: 2)! // 2진수 -> 10진수
print(decimal)
/* 27 */
```

