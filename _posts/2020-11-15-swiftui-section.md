---
layout: post
title: "SwiftUI - Section" 
date: 2020-11-15
category: read 
excerpt: ""

---

## List - Section

> Section : List 에서 row의 묶음 개념

1. 기본구조 설명
2. Data를 원하는 표현 형식대로 어떻게 가공하는지

### section - header

```swift
List{
  Section(header: Text("header area")){
   	Text("A") //여기는 원래 ForEach 같은걸로 나타내겠죠
  	Text("A")
  	Text("A") 
  }
}
```

- header는 String이 아니고, View 프로토콜을 따르는 형태여야 함(ex: Text, Image, ...)

<img src="https://user-images.githubusercontent.com/28949235/99256475-ce66aa80-2858-11eb-99b9-b50fa31b07df.png" alt="image" width="300" />

### section - footer

```swift
List{
	Section(footer: Image(systemName: "trash")){
		Text("A")
		Text("A")
		Text("A")
	}
}
```

<img src="https://user-images.githubusercontent.com/28949235/99256696-200f3500-2859-11eb-9fe1-54623db97579.png" alt="image" width="300" />

> header, footer를 같이 넣고 싶으면  
> **Section(header: _, footer: _, content: () -> _)** 사용



## 데이터 가공하기

데이터를 서버에서 받아오는 경우엔 잘 구분해서 데이터를 줄거기때문에 별 문제가 없겠지만(parsing만 잘 하면 됨)  
데이터를 내가 만들어야 하는 경우엔 ?

### 모델 만들기

```swift
struct Animal: Identifiable {
  let id = UUID() //View 쪽에서 사용하기 위한 고유한 ID 생성
	let name: String
  let index: Int //정렬을 위한 인덱스 (넘버링)
}
```

### 모델 구현하기

```swift
var animalList = [
  Animal(name: "dog", index: 1),
  Animal(name: "cat", index: 7),
  Animal(name: "dog", index: 6),
  Animal(name: "dog", index: 4),
  Animal(name: "cat" index: 5),
  Animal(name: "dog", index: 3),
  Animal(name: "bird", index: 2)
]
```

이렇게 배열 형태로 데이터가 있는 모델이 있다고 할 때, 저번 강의처럼 그냥 ForEach 써서 구현해보면

### ForEach로 나타내기

```swift
List {
  ForEach(animalList){ animal in
    HStack{
      Text("\(animal.name)")
      Text("\(animal.index)")
    }
  }
}
```

> 이렇게 할 경우 그냥 배열 순서대로 쭈루룩 나옴

### Section에 구분해서 나타내고 싶을 땐?

> dog는 dog대로.. cat은 cat대로





