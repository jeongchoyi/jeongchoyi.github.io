---
layout: post
title: "알고리즘을 위한 Swift 정리" 
date: 2021-08-02
category: read 
excerpt: ""


---

# 알고리즘을 위한 Swift 정리

> 알고리즘 풀이에 자주 쓰이는 Swift 함수 등등 아카이브

## ✨ 목차 ✨

[입출력](#io)  [배열](#array) [map,filter,reduce](#map-filter-reduce) [문자열 String](#string) [Character](#character) [수학](#math)

## 입출력<a id="io"></a>

**사용자 입력 받기**

> 입력이 여러 줄일 땐 readLine을 여러 번 써야 됨

* 기본 입력

```swift
var input = readLine()! // String type
```

```swift
var num = Int(readLine()!)! // Int 형 숫자 한 개 입력 받기
```

* 한 줄에 **공백과** 여러 개의 input이 올 땐 이렇게. string 배열로 바로 mapping된다.

```swift
let input = readLine()!.split(separator: " ").map{String($0)}
```

* mapping 할 때, Substring에서 Int 로 ( `Int($0)` )하는 것 보다 
  Substring → String → Int 로 ( `Int(String($0))!` ) 하는게 더 빠르다.

```swift
var nums = Array(readLine()!).map {Int(String($0))!}
```



## 배열<a id="array"></a>

* 2차원 배열

```swift
var arr = [[Int]]()

// 2차원 배열 1차원 배열로 만들기
var arr = [[1,2,3],[2,3],[4]] 
let flatten = arr.flatMap { $0 } // [1, 2, 3, 2, 3, 4] 
let reduced = arr.reduce([], +) // [1, 2, 3, 2, 3, 4] 
let joined = Array(arr.joined()) // [1, 2, 3, 2, 3, 4]
```

* 배열 만들기

```swift
var arr = Array(repeating: 1, count: 5) // [1,1,1,1,1]
var arr = Array(1...3) // [1,2,3]
```

* 배열 정렬

```swift
arr.sort() // 원본 순서 변경 (오름차순) [1,2,3]
arr.sorted() // 정렬한 배열을 반환
arr.sort(by: >) // 원본 순서 변경 (내림차순) [3,2,1]
```

* 인덱스에 따라 원소 다루기

```swift
arr.firstIndex(of: 3)! // 해당 원소 처음 만나는 인덱스 찾기
arr.remove(at: 2) // 해당 인덱스 원소 지우기
arr.append(6) // 맨 뒤에 새 원소 추가
arr.removeFirst() // 맨 앞 원소 지우고 반환
arr.removeLast() // 맨 뒤 원소 지우고 반환 (옵셔널 아님:원본 배열 비어있으면 절대 안 됨)
arr.popLast()! // 맨 뒤 원소 지우고 반환 (옵셔널)
arr.insert(3, at: 2) // 특정 인덱스(2)에 원소(3) 넣기
arr.contains(3) // 특정 원소 있는지 확인 (true/false)
arr.reverse() // 순서 반전
arr.first! // 첫 원소 반환
arr.removeAll() // 모든 원소 지우기
arr.removeAll(where: { $0 % 2 == 0}) // 조건을 만족하는 모든 원소 지우기
arr.swapAt(_:, _:) // 원소 스왑
```

* 최대값, 최소값

```swift
var min = arr.min()!
var max = arr.max()!
```

* 순회하며 원소와 인덱스 참조 - enumerated()

```swift
for (index, value) in arr.enumerated() {
    print("\(index), \(value)")
}
```

* 인덱스로 순회 (정방향, 역방향)

```swift
// 정방향
// count 사용
for i in 0..<arr.count {
    print(arr[i])
}

// indices 사용
for i in arr.indices {
    print(arr[i])
}
```

```swift
// 역방향
for i in (0..<arr.count).reversed() {
    print(arr[i])
}

for i in arr.indices.reversed() {
    print(arr[i])
}
// 아니면 배열 자체를 뒤집고 정방향 순회 해도 됨
```

* 배열 각 요소의 수 세기(Counter)

```swift
let arr = ["one", "two", "three", "four", "one"] 
var counter = [String: Int]() 
arr.forEach { counter[$0, default: 0] += 1 } 
print(counter) // ["three": 1, "one": 2, "two": 1, "four": 1]
```



## map<a id="map-filter-reduce"></a>

> 모든 원소에 적용

```swift
var arrInt = arrStr.map { Int($0)! } // [1,2,3] 원소를 모두 Int형으로 변환
```



## filter

> 조건이 참인 원소만 필터링

```swift
var arr = [1,2,3,4,5]
var arrEven = arr.filter { $0 % 2 == 0 } // [2, 4]
var arrOdd = arr.filter { $0 % 2 == 1 } // [1, 3, 5]
```

* 배열 특정 요소 개수 세기

```swift
let arr = ["D", "D", "R", "D"] 
arr.filter { $0 == "D" }.count // 3
```



## reduce

* sum

```swift
var arr = [1, 2, 3]
arr.reduce(0, +) // 6 // 모든 원소 더하기 (0은 초기값)
arr.reduce(0, -) // -6 // 모든 원소 빼기
```



## String<a id="string"></a>

* ch를 cnt만큼 반복한 String 만들기

```swift
String(repeating: ch, count: cnt)
```

* String을 한 자리씩 array로 split 하기

```swift
for i in str.indices {
    print(repeatString[i]) // 한 줄에 한 글자씩 print됨
}
```

* 특정 문자 기준으로 분리하기 - split()

```swift
var strings = str.split(separator: " ") // 공백 기준으로 분리
```

* 문자열 배열을 하나의 문자열로 합치기 - joined()

```swift
let arrHello = ["Hello", "World", "!"]
var hello = arrHello.joined() // "HelloWorld!"
hello = arrHello.joined(separator: " ") // Hello World !
hello = arrHello.joined(separator: ", ") // Hello, World, !
```

* 대,소문자 변경 - capitalized, uppercased(), lowercased()

```swift
var str = "abcdef"
str = str.capitalized  // 첫번째만 대문자로 변경 "Abcdef"
str = str.uppercased() // 전체 대문자로 변경 "ABCDEF"
str = str.lowercased() // 전체 소문자로 변경 "abcdef
```

* 문자열 포함 체크 - contains()

```swift
str.contains("a")  // true/false
```

* 특정 문자를 다른 문자로 변경한 문자열 반환

```swift
var str2 = str.replacingOccurrences(of: "a", with: "b")
print(str)  // 원본에 영향 없음
print(str2) // bbcdef
```

* String 인덱스 다루기

```swift
var str = "abcdef"

// 문자열 원소 접근
//str[0] // 직접 접근 안됨. String 인덱스로 접근해야함
str[str.startIndex] // "a"

// 3번째 문자 가져오기
let index = str.index(str.startIndex, offsetBy: 3 - 1)
str[index] // "c"

// 일정 범위의 문자열만 가져오기
let sub = str[str.startIndex...index] // "abc"

// 특정 원소의 인덱스
str.firstIndex(of: "c")
```

* 문자열 자르기

```swift
var s = "HelloWorld" 
var firstIndex = s.index(s.startIndex, offsetBy: 0) 
var lastIndex = s.index(s.endIndex, offsetBy: -5) 
// "Hello" 
var v = s[firstIndex ..< lastIndex]
```

```swift
// "Hello" 
var prefix = s.prefix(5) 
// "World" 
var suffix = s.suffix(5)
```



## Character<a id="character"></a>

### 아스키 코드

```swift
let asciiNum = Character(input[0]).asciiValue!
```



## 수학<a id="math"></a>

```swift
sqrt(4.0) // 제곱근(2.0) // Double 입력, Double 반환
pow(2.0, 3.0) // 거듭제곱(8.0(2^3)) // Double 입력, Double 반환
String(format: "%.2f", value) // 소수점 아래 자리수 제한, String 반환
abs(-11) // 11, 절대값
ceil(6.3) // 7.0 // 올림
floor(4.3) // 4.0 // 내림
round(5.5) // 6.0 // 반올림
```



## 🔗 출처

[kyungminleedev](https://kyungminleedev.github.io/notes/SwiftForAlgorighms/) [thoonk](https://thoonk.tistory.com/2)

