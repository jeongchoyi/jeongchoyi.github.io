---
layout: post
title: "Render Loop을 ViewController, View Life Cycle과 살펴보기" 
date: 2021-06-09
category: read 
excerpt: ""

---

# Render Loop을 VC, View Life Cycle과 살펴보기

[목차](https://iamcho2.github.io/2021/06/08/master-of-ios-life-cycle)

## 서론

> `viewDidLayoutSubviews()` 를 오버라이딩 해서 구현해야 할 일이 생겼는데,  
> 이렇게 그냥 써도 되는 것인가!? 라는 생각이 들어서 찾아본 내용 정리

MOMO를 개발할 때 해찌랑 [DispatchQueue 관련 실험](https://iamcho2.github.io/2021/01/06/dispatch-queue-with-haejji) 을 하면서  
**viewWillAppear**, **viewDidAppear** 사이  
**viewWillLayoutSubViews**, **viewDidLayoutSubViews** 요 두 놈이 있다는 것은 알게 되었지만,  
그리고 `DispatchQueue.main.async `가 그 후에 작업을 하는 것도 알게 되었지만 !!

`viewDidLayoutSubviews()` 요놈은 View Life Cycle 이랑 다른 점은 무엇인지,  
`updateConstraints()` 이런건 뭔지!!!  
그리고 오버라이딩 해서 써도 되는 것인지 (!!)를 알아보려고 한다. 차근차근 ~

**[의식의 흐름](https://iamcho2.github.io/2021/06/08/master-of-ios-life-cycle)을 따라 정리할 예정~!**



## ViewController Life Cycle, View Life Cycle?

**둘은 다른 것 !!**   
당연 뷰 컨트롤러 위에 언제나(?) 뷰가 올라가 있으니까 헷갈렸던 것이다... ㄱ-

이 외에도 app life cycle도 있고... (당연히) life cycle이라고 다 같은 life cycle이 아니다...!  
우선 ViewController Life Cycle이랑 View Life Cycle을 분리해서, 순서대로 어떻게 흘러가는지 살펴보자

<img src="https://user-images.githubusercontent.com/28949235/121371532-951e9500-c978-11eb-8acd-0845e68c6218.png" alt="lifecycle" width=600px />

> 이 이후에 `viewWillDisappear()`, `viewDidDisappear()`, `viewWillUnload()`, `viewDidUnload()`  도 있지만  
> 내가 궁금한 부분은 `viewDidAppear()` 전 까지이기 때문에 앞으로도 `viewDidAppear()` 까지만 표시할 것...( ◠‿◠ ) 

그렇다... 왼쪽이 **ViewController** Life Cycle, 오른쪽이 **View** Life Cycle !! (일부만 나타냄)  
둘은 엄연히 다른 것들이다.

그래그래 다른거 ㅇㅋㅇㅋ... 순서 ㅇㅋㅇㅋ...  
근데, `updateConstraints()`, `intrinsicContentSize()`, ... 이게 다 뭔데?!!?  
**언제 어디서 쓰는건데 !? 내가 오버라이딩 해서 써도 되는거야!?!?**



자자 그전에...

## Render Loop

> 언제 어디서 쓰나 봤더니, Render Loop이라는 게 있더라..~   
> [WWDC18 - High Performance Auto Layout](https://developer.apple.com/videos/play/wwdc2018/220/)에서는 **Render Loop**으로 소개된 내용.  
> ~~Layout Cycle이냐 Render Loop이냐 헷갈렸는데 아래의 내용은 Render Loop!!!~~

### Render Loop이 뭔데?

간단히 목적만 먼저 말하자면 **불필요한 일을 피하기 위해** 존재하는 것!  
매초 120번 정도 작동되는 프로세스인데, 각 프레임마다 모든 콘텐츠가 보여질 준비가 되었는지를 확실히 해준다.

자세히는 좀 더 뒤에 알아보도록 하고, 다시 Life Cycle로 돌아와서

<img src="https://user-images.githubusercontent.com/28949235/121371532-951e9500-c978-11eb-8acd-0845e68c6218.png" alt="lifecycle" width=600px />

위에 첨부한 요 순서들은 (init 제외) 3가지 단계로 구분될 수 있는데, 먼저 Render Loop의 3단계를 살펴보자

### Render Loop의 3가지 단계

1. **제약 조건 업데이트 (Update Constraints)**

   * Auto Layout의 제약조건(Constraints)를 업데이트 하는 단계.  
     말 그대로 "제약조건을 업데이트" 한 것이라서, 실제 뷰를 배치하는데에는 영향을 **안** 줌.
   * 뷰의 frame을 그것의 제약조건에 따라 계산하는 과정

   | Image                                                        | Description                                                  |
   | ------------------------------------------------------------ | ------------------------------------------------------------ |
   | <img src="https://user-images.githubusercontent.com/28949235/121385870-76260000-c984-11eb-94e0-f9bd2499f3a7.png" alt="image" width=200px /> | <span style="font-weight:normal">제약조건의 갱신은<br/>뷰 계층구조 (view hierarchy)를 따라 하위 뷰에서 상위 뷰 방향으로 횡단하며 이루어짐.<br/>(leaf most views -> window 방향, **Bottom-Up**)<br/><br/>`updateConstraints()`를 필요로 하는 모든 뷰가 `updateConstraints()`를 받음. <br/><br/><br/>(요 동작은 자동으로 일어나지만, <br />상황에 따라 유저가 trigger를 해 줘야 할 때가 있는데,  <br />요 내용은 뒤에서 살펴보는걸로 !!) </span> |

   

2. **레이아웃 업데이트 (Layout)**

   * 제약조건을 바탕으로 레이아웃(뷰의 프레임 수치 값)을 업데이트 하는 단계.  
     Update Constraints 단계에서 계산된 것들을 바탕으로 뷰의 프레임이 업데이트 됨.

   | Image                                                        | Description                                                  |
   | ------------------------------------------------------------ | ------------------------------------------------------------ |
   | <img src="https://user-images.githubusercontent.com/28949235/121388189-3cee8f80-c986-11eb-935e-aa11f966b275.png" alt="image" width=200px /> | <span style="font-weight:normal">이 프로세스는 전 단계와는 반대 방향으로<br />모든 뷰가 `layoutSubviews()`를 받는다. <br />(window -> leaves 방향, **Top-Down**) <br /><br /><br />( 요 단계가 `layoutSubviews()`를 오버라이딩 한다거나, <br />`setNeedsLayout()`, `layoutIfNeeded()` 요놈들이 등장하는 타이밍인데,<br />이것들도 뒤에서 자세히 살펴보는걸로 ,,~~)</span> |

   

3. **디스플레이 (Display)** ~~(Render, Draw라고도 하는 듯)~~

   * 픽셀들을 스크린으로 가져오는 단계!
   * 뷰를 Layout 단계에서 구한 수치값을 사용해 스크린에 그리는 단계.

   | Image                                                        | Description                                                  |
   | ------------------------------------------------------------ | ------------------------------------------------------------ |
   | <img src="https://user-images.githubusercontent.com/28949235/121389371-4b897680-c987-11eb-800b-e339d5b087fd.png" alt="image" width=200px /> | <span style="font-weight:normal">Layout 단계와 같은 방향으로 진행되면서,<br />모든 뷰들은 필요시 draw()를 받는다.<br /><br />요 `draw()`가 원래는 `drawRect()`였는데, rect가 매개변수로 빠지면서 `draw()`가 됐다.<br />(검색하면 `drawRect()`라고 많이 나옴)</span> |

   

자 이렇게 Render Loop의 3단계에 대해서 알아봤으니, Life Cycle 함수들을 3단계로 구분해보자면

![lifecycle-Recovered](https://user-images.githubusercontent.com/28949235/121391424-504f2a00-c989-11eb-8566-9142018c66a1.png)

요렇게 !!!



### 다음 포스팅에서는...

 🤷‍♀️ 3단계가 있는거 알겠고, 그 안에 무슨 함수들이 있는지 알겠어.  
근데 **언제 어디서 쓰는건데 !? 내가 오버라이딩 해서 써도 되는거냐고!!!**  
에 대한 답을 찾으러 떠나봅시다...... 💪 ( ◠‿◠ )

