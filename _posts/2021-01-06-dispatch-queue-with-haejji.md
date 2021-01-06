---
layout: post
title: "DispatchQueue 궁금증 해결" 
date: 2021-01-06
category: read 
excerpt: ""

---

# DispatchQueue 궁금증 해결 완료...ㅠㅁㅠ!!!!!!

우선!

* **viewDidLoad**는 main thread에서 수행되는 게 맞다.

* View LifeCycle에서 **viewDidLoad** - **viewWillAppear** - **viewDidAppear** 순인데,

  **viewWillAppear**, **viewDidAppear** 사이에

  **viewWillLayoutSubViews**, **viewDidLayoutSubViews** 요 두 놈이 있다.

  그 안에서 레이아웃 수정을 다 한 후에 우리가 레이아웃 수정을 해야 제대로 반영이 된다.



예를 들어서, 오빠 코드의 viewDidLoad()에는

```swift
self.scrollView.frame.origin.y = -self.statusBarHeight!
```

이 있었는데, **viewDidLoad()**에서 실행했을 땐 안 되고, 그 안에서

```swift
DispatchQueue.main.async {
		self.scrollView.frame.origin.y = -self.statusBarHeight!
}
```

요런 식으로 DispatchQueue를 사용해서 main thread에서 UI 변경 작업이 수행되도록 해야지  
우리가 원하는 대로 제대로 코드가 동작했었다.
아니면 **viewDidAppear()** 내부에 저 line을 넣어야지 동작했었다.



### 우리가 오해했던 것

[이 stackoverflow 질문](https://stackoverflow.com/questions/44293454/dispatchqueue-main-async-in-viewdidload) 을 보고, 아래와 같이 생각을 했다.

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    firstSelector() //"First selector fired" 출력
    DispatchQueue.main.async {
        self.secondSelector() //"Second selector fired" 출력
    }
    for i in 1...10 {
        print(i)
    }
    thirdSelector() //"Third selector fired" 출력
}
```

이렇게 실행했을 때

```swift
First selector fired
1
2
3
4
5
6
7
8
9
10
Third selector fired
Second selector fired //DispatchQueue.main.async 블럭 내의 함수 출력
```

이렇게 출력되는걸로 봐서,

![queue](https://user-images.githubusercontent.com/28949235/103771957-0970a780-506c-11eb-9564-d06fb928e923.png)

queue에 요런 순서로 들어간 것 까지는 이해했는데,  그러면 아래와 같이 코드를 작성해도 똑같이 동작하니까,  
(DispatchQueue 사용 X)

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    firstSelector() //"First selector fired" 출력
    for i in 1...10 {
        print(i)
    }
    thirdSelector() //"Third selector fired" 출력
    self.secondSelector() //"Second selector fired" 출력
}
```

오빠 코드에 있던

```swift
self.scrollView.frame.origin.y = -self.statusBarHeight!
```

요놈도 viewDidLoad() 맨 아래로 내리면 똑같이 동작하지 않을까,,, 라고 생각했다.  
UI수정이라 뭔가 다르겠다고 생각은 했지만, viewDidLoad() 도 main thread니까.  
근데 제대로 동작 안 함.



### viewDidAppear에 넣어도 동작하고,  

### viewDidLoad()에서 DispatchQueue 해도 동작?

여기서 답을 찾았다..!

**viewDidLoad()**에서 `DispatchQueue.main.async` 을 해준다고 해서  
viewDidLoad() 내 함수의 호출들이 다 queue에 들어가고 나서  
`DispatchQueue.main.async` 블록 내의 호출이 그 **바로 다음**으로 queue에 들어가는 것이 아니였다..!!!

![](https://user-images.githubusercontent.com/28949235/103772836-789acb80-506d-11eb-9a1d-8155ec2366e4.png)

**viewDidLoad()** 내에

```swift
DispatchQueue.main.async {
		self.scrollView.frame.origin.y = -self.statusBarHeight!
		print("UI 변경!")
}
```

이렇게 DispatchQueue 를 사용해주고,  
위와 같이 각 Life cycle 함수 내부에 print문을 통해 console에 출력을 해주면,  
실행 순서가 다음과 같다.

**viewDidLoad -> viewWillLayoutSubviews -> viewDidLayoutSubviews -> "UI 변경!" 출력 -> viewDidAppear**

**viewDidLayoutSubViews** 후에 우리가 레이아웃 수정을 해야 제대로 반영이 된다는 건 알았지만,  

### 우리가 깨달은 건,,,

**viewDidLoad()**에서 DispatchQueue.main.async를 한다고 해서 **viewDidLoad()** 안에서만 동작하는 게 아니라  
UI 작업이 반영되는 타이밍까지 기다렸다가 해당 코드를 실행해주는 식으로 **우선순위가 정해진다**는 것!

요 부분은 QoS의 `user interactive` 관련인 것 같다. (내생각)

암튼... dispatchQueue 진짜 똑똑하네잉..

