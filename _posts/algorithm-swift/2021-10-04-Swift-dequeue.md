---
layout: post
title: "Swift로 효율적인 dequeue 구현하기 (포인터, 투스택 큐)" 
date: 2021-10-04
category: read 
excerpt: ""
---

# Swift로 효율적인 dequeue 구현하기 for 코테

Swift는 Queue가 구현이 되어 있지 않아서, 직접 배열로 구현을 해야한다.    
`append()`랑 `removeFirst()`를 사용해서 알고리즘 문제를 풀다가,  
`removeFirst()` 때문에 시간초과가 나는 일이 발생했다.

이는 `removeFirst()`가 O(N)의 시간복잡도를 가지기 때문인데,  
아직 기초 단계일 때 시간복잡도를 챙긴 queue 사용법을 익혀버리고  
간단한 문제, 복잡한 문제 모두 이 방법을 사용하려고 한다.

블로그를 찾아보면 struct로 아예 만들어서 사용하던데,   
그걸 다 외우고 코테에서 문제 풀기 전에 좌라락 쓸 자신은 없어서 따로 struct로는 사용하지 않을 것 같다.  

```swift
var queue: [Int] = []
```

이런 배열이 있다고 치고 시작!

<img src="https://user-images.githubusercontent.com/28949235/135823432-f3521621-ec9e-46d9-a47c-c2b42bcb55d7.jpeg" alt="IMG_FF32BFE2AA66-1" width=500 />

### enqueue

enqueue는 간단하다. 그냥 append 해 주면 된다.

```swift
queue.append(1)
```

배열의 마지막 위치에 원소가 append되고, O(1)의 시간복잡도를 가진다.

### dequeue

```swift
queue.removeFirst()
```

는 O(N)의 시간복잡도를 가진다.  
맨 처음의 원소를 지우고, 남은 모든 원소를 한 칸씩 shift해야 하기 때문이다.  
위에서도 언급했듯, 요게 문제가 된다... 큰 큐일수록 더더욱 !!

이를 해결하기 위해 여러 방법이 있다.  
연결 리스트를 사용하는 방법도 있고, 버퍼, 포인터, 스택 두 개 사용하기 등...

이중에서 포인터랑 스택 두 개를 사용하는 방법! 이렇게 두가지를 알아보려고 한다.  
연결리스트는 Node 클래스도 만들어야되고, 공간 복잡도가 크다. 메모리 접근 시간도 느리다.  
버퍼는.. 그냥 .. 뭔가 .. 네..

포인터는 구현이 간단하고,  
스택 두 개를 사용하는 방법은 나중에 Deque(덱) 구현할 때 써먹을건데 미리미리.. 공간복잡도 면에서도 좋다.

## 포인터를 사용해 dequeue 구현하기

> 그림이랑 같이 봅시다

<img src="https://user-images.githubusercontent.com/28949235/135829702-de2df3a3-9be6-4f05-9bd4-f9bf132fa740.jpeg" alt="IMG_87331308DF13-1" width=500 />

이런 Queue가 있다고 하자.

```swift
var queue: [Int] = [0, 1, 2, 3, 4] //index랑 value랑 같은 배열임 !!
// 본인(element)의 index값이 담긴 배열
```



<img src="https://user-images.githubusercontent.com/28949235/135829480-7ad8112a-e7ff-430c-929d-32de37b775be.jpeg" alt="IMG_87331308DF13-1 2" width=500 />

head라는 Int형 변수를 만들어서, 맨 앞 원소의 index값으로 초기화 해 준다. (`0`)

```swift
var head: Int = 0
```

<img src="https://user-images.githubusercontent.com/28949235/135829491-84e84d46-73ab-4445-9f12-7d3b31b02f87.jpeg" alt="IMG_87331308DF13-1 3" width=500 />

이제 dequeue를 해 줄건데,

<img src="https://user-images.githubusercontent.com/28949235/135829499-242c77c0-80c0-483e-8d06-a95119b6b97f.jpeg" alt="IMG_87331308DF13-1 4" width=500 />

할 일은 2가지다.

1. 맨 앞 원소를 nil로 만들기
2. head 옮겨주기

```swift
queue[head] = nil // 맨 앞 원소를 nil로 만들기
head += 1 // head 옮겨주기
```

이렇게 하면 O(N) 이었던 dequeue를 O(1)로 구현할 수 있다.

그런데 이 때, 고려해야 할 것이 있다. **queue가 비어있을 때** !!  
위의 할 일을 해 주기 전에, 먼저 검사해주면 된다.

```swift
guard let element = queue[head] else { return nil }
queue[head] = nil // 맨 앞 원소를 nil로 만들기
head += 1 // head 옮겨주기
return element
```

필요하면 head도 검사해주면 된다,,

<img src="https://user-images.githubusercontent.com/28949235/135829501-c5a56942-c4ba-4e67-973c-0f1f480e7019.jpeg" alt="IMG_87331308DF13-1 5" width=600/>

배열을 사용하는 데에서 오는 문제점도 존재한다.  
큰 ~~~ 배열일수록, 또는 enqueue가 많이 되는 배열일수록 메모리 문제가 생긴다.  
(Array append 시, 꽉 찬 배열에 append를 하면, 배열 크기를 2배로 늘리고 append를 한다)

적절한 때에 nil로 dequeue된 n개를 **removeFirst(n)** 해 주면 된다.

## 스택 두 개를 사용해 queue 구현하기

<img src="https://user-images.githubusercontent.com/28949235/135833577-4fedff48-6efa-4047-be5d-230757ad1dab.jpeg" alt="IMG_A6781AED2EB4-1" width=400 />

이렇게 dequeue용, enqueue용 두 개의 stack을 사용하는 방법이다.

<img src="https://user-images.githubusercontent.com/28949235/135833565-1a9fa103-231f-442c-a146-57124d467218.jpeg" alt="IMG_A6781AED2EB4-1 2" width=400 />
이해를 돕기 위해... 두 stack을 눕혀보면 요런 느낌!

두 Stack (배열로 구현)을 만들어준다.

```swift
var leftStack: Int = [] // dequeue용
var rightStack: Int = [] // enqueue용
```

### enqueue

enqueue는 동일하다. enqueue용 스택인 rightStack에 append 해 주면 된다.

```swift
rightStack.append(1)
```

<img src="https://user-images.githubusercontent.com/28949235/135836914-e415a2a6-d38a-43e5-b889-a7964e9423a9.png" alt="image" width=500 />

enqueue는 rightStack에 하니까, queue랑 같이 보면 요런 느낌.

### dequeue

<img src="https://user-images.githubusercontent.com/28949235/135836914-e415a2a6-d38a-43e5-b889-a7964e9423a9.png" alt="image" width=500 />

위에서 본 그림에 이어서, dequeue를 하는 과정을 살펴보자.

<img src="https://user-images.githubusercontent.com/28949235/135837069-7dc43c55-770e-4951-8ea5-425286c380dc.png" alt="image" width=500 />

이 상태에서, dequeue를 해야 하는데 지금은 dequeue용 stack인 leftStack이 비어있다.

<img src="https://user-images.githubusercontent.com/28949235/135837191-b44f358b-7e0e-4e15-b983-a61e7c45aba9.png" alt="image" width=500 />

leftStack이 비어있지 않다면, 바로 dequeue를 해 주면 되지만,  
비어있을 경우를 대비해 isEmpty로 조건문을 만들어 준다.

```swift
if leftStack.isEmpty {
  
}
```

<img src="https://user-images.githubusercontent.com/28949235/135837451-736856c4-bd6c-4eb0-9196-61812e47fc4c.png" alt="image" width=500 />

leftStack을 채워줄건데,  Swift에서 Array의 `reversed()` 가 O(1)의 시간복잡도를 가진다는 데에 이 방법을 사용하는 이유가 있다.  
rightStack을 뒤집어준 후, leftStack에 대입한다.

```swift
if leftStack.isEmpty {
  leftStack = rightStack.reversed()
}
```

leftStack이 비어있을 때에만 rightStack의 값을 옮겨주니까, 값이 중복되거나 할 위험이 없다.

<img src="https://user-images.githubusercontent.com/28949235/135837759-035ac23a-0903-4350-9da9-953a9f2dd9bf.png" alt="image" width=600 />

현재 leftStack과 rightStack은 이 상태!  
(위 그림에서 leftStack의 top 방향을 이해하기 쉽게 바꿔 그린 상태, index 참고)  
rightStack의 값을 leftStack으로 옮겨줬으니까 rightStack을 비워준다.

<img src="https://user-images.githubusercontent.com/28949235/135837886-f0bda037-8e99-4e8b-8fdc-0b4add819c7e.png" alt="image" width=600 />

```swift
if leftStack.isEmpty {
  leftStack = rightStack.reversed()
  rightStack.removeAll()
}
```

어차피 rightStack은 enqueue용 stack이라서, 앞으로 enqueue될 것만 기다리면 되기 때문에  
leftStack으로 값을 다 옮겨도 다시 가져와야 하거나 할 일이 없다.

이제 leftStack이 비어있지 않은 상태가 됐다.

<img src="https://user-images.githubusercontent.com/28949235/135838116-41ba27e5-709c-47f0-a907-07a316633ed6.png" alt="image" width=600 />

이제 leftStack의 top에 있는 element를 `popLast()`만 해 주면 된다. 얘도 O(1)!!

```swift
if leftStack.isEmpty {
  leftStack = rightStack.reversed()
  rightStack.removeAll()
}
leftStack.popLast()
```

그런데, 이 방법을 사용할 때에는 하나의 queue를 two stack으로 분리해 놓은 것이기 때문에 주의할 게 있다.

### isEmpty

보통은

```swift
if queue.isEmpty { }
```

하면 되지만, 이렇게 투 스택으로 queue를 구현해놓은 상태에서는

```swift
if leftStack.isEmpty && rightStack.isEmpty { }
```

두 개의 stack 모두 검사해줘야 한다.

### peek

queue의 head에 있는 값을 보고싶다면 보통은

```swift
var head = queue.first
```

로 받아올 수 있었지만, two stack queue에서는 leftStack이 비었는지 아닌지를 검사 해 줘야한다.

```swift
var head = leftStack.isEmpty ? rightStack.first : leftStack.last
```

<img src="https://user-images.githubusercontent.com/28949235/135839387-e8b17c7f-d592-4fbe-b538-980f5d6f94b5.png" alt="image" width=600 />



rightStack이 아니고 leftStack이 기준이 되는 이유는  
두 스택 모두 값이 있을 때, 가장 먼저 들어온 값은 **dequeue전용 스택인 leftStack에 존재**하기 때문이다 !!!



이렇게 두 가지 방법을 살펴봤는데,  
뭘 쓸지는... 우선 문제를 많이 풀어봐야 알 것 같다...!!!!!!!!!!!

#문제풀러갑시다.
