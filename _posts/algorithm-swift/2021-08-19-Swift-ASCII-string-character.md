---
layout: post
title: "Swift 문자, 문자열과 ASCII 코드" 
date: 2021-08-19
category: read 
excerpt: ""


---

# Swift 문자, 문자열과 ASCII 코드

> 알고리즘 풀이를 위한 문자열 개념 공부

### 문자 -> ASCII

```swift
문자열의 유니코드 표현에 접근하기
let cafe = "Cafe\u{301} du 🌍"
print(cafe.count)
// Prints "9"
print(Array(cafe))
// Prints "["C", "a", "f", "é", " ", "d", "u", " ", "🌍"]"
유니코드 스칼라 보기 (UTF-32 코드 단위와 동일함)

print(cafe.unicodeScalars.count)
// Prints "10"
print(Array(cafe.unicodeScalars))
// Prints "["C", "a", "f", "e", "\u{0301}", " ", "d", "u", " ", "\u{0001F30D}"]"
print(cafe.unicodeScalars.map { $0.value })
// Prints "[67, 97, 102, 101, 769, 32, 100, 117, 32, 127757]"
```

문자열의 unicodeScalars 프로퍼티는 유니코드 스칼라 값의 collection이다.
( 이 때, é에서 e(101)와 악센트(769)가 분리되어 count가 10이 된다.)

unicodeScalars를 사용하면 asciiValue보다 속도가 조금 빠르다고 한다. (문제풀이에 영향 갈 정도는 아님)

문자열을 ASCII코드의 배열로 받기 위해서는 Character타입에 접근해서 사용하면 된다. (map 부분 참고)

```swift
let input = readLine()!.split(separator: " ").map { String($0) }
let asciiNum = Character(input[0]).asciiValue!
print(asciiNum)
```

asciiValue는 사실 unicodeScalars의 래퍼라고 한다,,, Swift 5 이상 지원!

### ASCII -> 문자

As a character:

```swift
let c = Character(UnicodeScalar(65))
```

Or as a string:

```swift
let s = String(UnicodeScalar(UInt8(65)))
```

Or another way as a string:

```swift
let s = "\u{41}"
```

걍 문자열 보간법 사용해도 된다. ;;
