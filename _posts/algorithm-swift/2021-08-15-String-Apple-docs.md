---
layout: post
title: "String - Apple 공식 문서 읽기 (Swift)" 
date: 2021-08-15
category: read 
excerpt: ""


---

# String - Apple 공식 문서 읽기 (Swift)

> 알고리즘 풀이를 위한 문자열 개념 공부

Swift PS하는 데 걸리는 것 중에 가장 악명이 높은 String...을 정복해보는 시간 ~!!  
특히, index 관련이나 substring 관련 개념이 아직 부족하다.  우선 [Apple Docs](https://developer.apple.com/documentation/swift/string) 부터 보자.

### 문자열 결합

연결 연산자 (+) 를 사용해서 문자열을 결합할 수 있음.

```swift
let longerGreeting = greeting + " We're glad you're here!"
// longerGreeting == "Welcome! We're glad you're here!"
```

### 문자열 비교

== 나 부등호(<, >=)를 사용해서 문자열을 비교할 수 있음.  
이때, 비교는 항상 유니코드 표준 표현에 따라 수행됨. 표현 방법이 달라도 유니코드면에서 같으면 같은 것.

```swift
let cafe1 = "Cafe\u{301}"
let cafe2 = "Café"
print(cafe1 == cafe2)
// Prints "true"
```

### 문자열 요소에 접근하기

**각 요소는 `Character` 형**임.

ex) 긴 문자열의 첫번째 단어를 검색하는 법  
(공백을 검색한 후 해당 지점까지 문자열의 접두사에서 하위 문자열을 만들면 됨.)

```swift
let name = "Marie Curie"
let firstSpace = name.firstIndex(of: " ") ?? name.endIndex
let firstName = name[..<firstSpace]
// firstName == "Marie"
```

이 때, **firstName은 `Substring` 형**임.  
`Substring` 형은 원래 문자열의 저장공간을 같이 쓰고,  
String과 동일한 interface를 제공함.

### 문자열의 유니코드 표현에 접근하기

```swift
let cafe = "Cafe\u{301} du 🌍"
print(cafe.count)
// Prints "9"
print(Array(cafe))
// Prints "["C", "a", "f", "é", " ", "d", "u", " ", "🌍"]"
```

**유니코드 스칼라 보기** (UTF-32 코드 단위와 동일함)

```swift
print(cafe.unicodeScalars.count)
// Prints "10"
print(Array(cafe.unicodeScalars))
// Prints "["C", "a", "f", "e", "\u{0301}", " ", "d", "u", " ", "\u{0001F30D}"]"
print(cafe.unicodeScalars.map { $0.value })
// Prints "[67, 97, 102, 101, 769, 32, 100, 117, 32, 127757]"
```

문자열의 `unicodeScalars` 프로퍼티는 유니코드 스칼라 값의 collection이다.  
( 이 때, é에서 e(101)와 악센트(769)가 분리되어 count가 10이 된다.)

## 함수

### 문자열 생성

```swift
init(Character) : 주어진 문자를 포함하는 문자열 생성
init(Substring) : 지정된 부분 문자열에서 새 문자열을 만듦
init(repeating: String, count: Int) : 지정된 횟수만큼 반복되는 지정된 문자열을 나타내는 새 문자열을 만듦
init(repeating: Character, count: Int) : 지정된 횟수만큼 반복되는 지정된 문자를 나타내는 문자열을 작성
```

### 문자열 검사

```swift
var isEmpty: Bool : 문자열에 문자가 없는지 여부
var count: Int 문자열의 문자 수
```

### 문자열 및 문자 추가

```swift
func append(String) : 주어진 문자열을 이 문자열에 추가
func append(Character) : 문자열에 주어진 문자를 추가
func append(contentsOf: String)
func append(contentsOf: Substring)
```

### 문자 삽입

```swift
func insert(Character, at: String.Index) : 지정된 위치에 새 문자를 삽입합니다.
func insert(Character, at: Index) : 컬렉션의 지정된 위치에 새 요소를 삽입합니다.
func insert<C>(contentsOf: C, at: Index) : 컬렉션의 지정된 위치에 시퀀스의 요소를 삽입합니다.
func insert<S>(contentsOf: S, at: String.Index) : 지정된 위치에 문자 모음을 삽입합니다.
```

### 부분 문자열 바꾸기

```swift
func replaceSubrange<C>(Range<String.Index>, with: C)
: 지정된 범위 내의 텍스트를 지정된 문자로 바꿉니다.
func replaceSubrange<C, R>(R, with: C)
: 요소의 지정된 하위 범위를 지정된 컬렉션으로 바꿉니다.
```

### 부분 문자열 제거

```swift
func remove(at: String.Index) -> Character : 지정된 위치의 문자를 제거하고 반환합니다.
func remove(at: Index) -> Character : 지정된 위치의 요소를 제거하고 반환합니다.
func removeAll(keepingCapacity: Bool) : 이 문자열을 빈 문자열로 바꿉니다.
func removeAll(where: (Character) -> Bool) : 주어진 술어를 만족하는 모든 요소를 제거합니다.
func removeFirst() -> Character : 컬렉션의 첫 번째 요소를 제거하고 반환합니다.
func removeFirst(Int) : 컬렉션의 시작 부분에서 지정된 수의 요소를 제거합니다.
func removeLast() -> Character : 컬렉션의 마지막 요소를 제거하고 반환합니다.
func removeLast(Int) : 컬렉션의 끝에서 지정된 수의 요소를 제거합니다.
func removeSubrange(Range<String.Index>) : 지정된 범위의 문자를 제거합니다.
func removeSubrange(Range<Index>) : 컬렉션에서 지정된 하위 범위의 요소를 제거합니다.
func removeSubrange<R>(R) : 컬렉션에서 지정된 하위 범위의 요소를 제거합니다.
func filter((Character) -> Bool) -> String
: 주어진 조건자를 만족하는 원래 컬렉션의 요소를 순서대로 포함하는 동일한 유형의 새 컬렉션을 반환합니다.
func drop(while: (Character) -> Bool) -> Substring
: 잠시 요소를 생략하여 서브 리턴 predicate반환 true하고 나머지 요소를 반환합니다.
func dropFirst(Int) -> Substring
: 지정된 수의 초기 요소를 제외한 모든 요소를 포함하는 하위 시퀀스를 반환합니다.
func dropLast(Int) -> Substring
: 지정된 수의 최종 요소를 제외한 모든 요소를 ​​포함하는 하위 시퀀스를 반환합니다.
func popLast() -> Character?
: 컬렉션의 마지막 요소를 제거하고 반환합니다.
```

### 대소문자 변경

```swift
func lowercased() -> String : 문자열의 소문자 버전을 반환합니다.
func uppercased() -> String : 문자열의 대문자 버전을 반환합니다.
```

### 부분 문자열 찾기

```swift
func hasPrefix(String) -> Bool
: 문자열이 지정된 접두사로 시작하는지 여부를 나타내는 부울 값을 반환합니다.
func hasSuffix(String) -> Bool
: 문자열이 지정된 접미사로 끝나는지 여부를 나타내는 부울 값을 반환합니다.
```

### 캐릭터 찾기

```swift
func contains(Character) -> Bool
: 시퀀스에 지정된 요소가 포함되어 있는지 여부를 나타내는 부울 값을 반환합니다.
func allSatisfy((Character) -> Bool) -> Bool
: 시퀀스의 모든 요소가 주어진 조건자를 충족하는지 여부를 나타내는 부울 값을 반환합니다.
func contains(where: (Character) -> Bool) -> Bool
: 시퀀스에 주어진 조건자를 만족하는 요소가 포함되어 있는지 여부를 나타내는 부울 값을 반환합니다.
func first(where: (Character) -> Bool) -> Character?
: 주어진 조건자를 만족하는 시퀀스의 첫 번째 요소를 반환합니다.
func firstIndex(of: Character) -> Index?
: 컬렉션에서 지정된 값이 나타나는 첫 번째 인덱스를 반환합니다.
func firstIndex(where: (Character) -> Bool) -> Index?
: 컬렉션의 요소가 주어진 조건자를 만족하는 첫 번째 인덱스를 반환합니다.
func last(where: (Character) -> Bool) -> Character?
: 주어진 조건자를 만족하는 시퀀스의 마지막 요소를 반환합니다.
func lastIndex(of: Character) -> Index?
: 컬렉션에서 지정된 값이 나타나는 마지막 인덱스를 반환합니다.
func lastIndex(where: (Character) -> Bool) -> Index?
: 주어진 술어와 일치하는 컬렉션의 마지막 요소 인덱스를 반환합니다.
func max() -> Character?
: 시퀀스의 최대 요소를 반환합니다.
func max<T>(T, T) -> T
func max(by: (Character, Character) -> Bool) -> Character?
: 주어진 술어를 요소 간의 비교로 사용하여 시퀀스의 최대 요소를 리턴합니다.
func min() -> Character?
: 시퀀스의 최소 요소를 반환합니다.
func min<T>(T, T) -> T
func min(by: (Character, Character) -> Bool) -> Character?
: 주어진 술어를 요소 간의 비교로 사용하여 시퀀스의 최소 요소를 리턴합니다.
```

### 부분 문자열 가져오기

```swift
subscript(Range<String.Index>) -> Substring
: 컬렉션 요소의 인접한 하위 범위에 액세스합니다.
subscript<R>(R) -> Substring
: 범위 식으로 지정된 컬렉션 요소의 연속 하위 범위에 액세스합니다.
subscript((UnboundedRange_) -> ()) -> Substring
func prefix(Int) -> Substring
: 컬렉션의 초기 요소를 포함하는 지정된 최대 길이까지 하위 시퀀스를 반환합니다.
func prefix(through: Index) -> Substring
: 컬렉션의 시작부터 지정된 위치까지의 하위 시퀀스를 반환합니다.
func prefix(upTo: Index) -> Substring
: 컬렉션의 시작부터 지정된 위치까지(포함하지 않음)까지의 하위 시퀀스를 반환합니다.
func prefix(while: (Character) -> Bool) -> Substring
: 반환할 때까지 초기 요소를 포함하는 하위 시퀀스를 predicate반환 false하고 나머지 요소 는 건너뜁니다.
func suffix(Int) -> Substring
: 컬렉션의 최종 요소를 포함하는 지정된 최대 길이까지 하위 시퀀스를 반환합니다.
func suffix(from: Index) -> Substring
: 지정된 위치에서 컬렉션 끝까지의 하위 시퀀스를 반환합니다.
```

### 문자열 분할

```swift
func split(separator: Character, maxSplits: Int, omittingEmptySubsequences: Bool) -> [Substring]
: 주어진 요소와 동일한 요소 주위에서 컬렉션의 가능한 가장 긴 하위 시퀀스를 순서대로 반환합니다.
func split(maxSplits: Int, omittingEmptySubsequences: Bool, whereSeparator: (Character) -> Bool) -> [Substring]
: 주어진 술어를 만족하는 요소를 포함하지 않는 컬렉션의 가능한 가장 긴 하위 시퀀스를 순서대로 반환합니다.
```

### 문자 및 바이트 가져오기

```swift
subscript(String.Index) -> Character : 지정된 위치의 문자에 액세스합니다.
var first: Character? : 컬렉션의 첫 번째 요소입니다.
var last: Character? : 컬렉션의 마지막 요소입니다.
```

### 문자열의 문자 반복

```swift
func forEach((Character) -> Void) 
// for- in루프 와 같은 순서로 시퀀스의 각 요소에 대해 주어진 클로저를 호출합니다 .
func enumerated() -> EnumeratedSequence<String>
// 쌍의 시퀀스( n , x )를 반환합니다 . 여기서 n 은 0에서 시작하는 연속적인 정수를 나타내고 x 는 시퀀스의 요소를 나타냅니다.
```

### 문자열의 문자 재정렬

```swift
func sorted() -> [Character]
: 정렬된 시퀀스의 요소를 반환합니다.
func sorted(by: (Character, Character) -> Bool) -> [Character]
: 주어진 술어를 요소 간의 비교로 사용하여 정렬된 시퀀스의 요소를 리턴합니다.
func reversed() -> ReversedCollection<String>
: 컬렉션의 요소를 역순으로 표시하는 뷰를 반환합니다.
```

### 인덱스 조작

```swift
var startIndex: String.Index
: 비어 있지 않은 문자열에서 첫 번째 문자의 위치입니다.
var endIndex: String.Index
: 문자열의 "끝 이후" 위치, 즉 마지막 유효한 아래 첨자 인수보다 하나 큰 위치입니다.
func index(after: String.Index) -> String.Index
: 주어진 인덱스 바로 뒤의 위치를 반환합니다.
func index(before: String.Index) -> String.Index
: 주어진 인덱스 바로 앞의 위치를 반환합니다.
func index(String.Index, offsetBy: String.IndexDistance) -> String.Index
: 주어진 인덱스로부터 지정된 거리에 있는 인덱스를 반환합니다.
func index(String.Index, offsetBy: String.IndexDistance, limitedBy: String.Index) -> String.Index?
: 거리가 지정된 제한 인덱스를 벗어나지 않는 한 지정된 인덱스에서 지정된 거리인 인덱스를 반환합니다.
func distance(from: String.Index, to: String.Index) -> String.IndexDistance
: 두 인덱스 사이의 거리를 반환합니다.
var indices: DefaultIndices<String>
: 컬렉션을 첨자화하는 데 유효한 인덱스(오름차순).
```



## 🔗 출처

[Apple Docs](https://developer.apple.com/documentation/swift/string)
