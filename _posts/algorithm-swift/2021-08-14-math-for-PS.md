---
layout: post
title: "알고리즘을 위한 수학 공식 정리 (Swift)" 
date: 2021-08-14
category: read 
excerpt: ""


---

# 알고리즘을 위한 수학 공식 정리 (Swift)

> 알고리즘 풀이에 자주 쓰이는 수학 공식 아카이브

### 최소공배수, 최대공약수

2개의 수의 **최대공약수** - 유클리드 호제법, **최소공배수** - 최대공약수를 통해 구하면 됨!
**유클리드 호제법**

> 두 개의 자연수 a와 b에서(단 a>b) a를 b로 나눈 나머지를 r이라고 했을때 
> GCD(a, b) = GCD(b, r)과 같고 r이 0이면 그때 b가 최대공약수이다. 라는 원리를 활용한 알고리즘
>
> **재귀로 풀기**
>
> ```swift
> func gcd(_ a: Int, _ b: Int) -> Int {
>     if b == 0 { return a }
>     else {
>         return gcd(b, a%b)
>     }
> }
> ```
>
> **반복문으로 풀기**
>
> ```swift
> func gcd(a: Int, b: Int) {
>   while(true) {
>     var r = a%b
>     it r == 0 { return b }
>     
>     a = b
>     b = r
>   }
> }
> ```

**최소공배수**

```swift
func lcm(_ a: Int, _ b: Int) -> Int {
    return a * b / gcd(a, b)
}
```



### 수열





## 🔗 출처

