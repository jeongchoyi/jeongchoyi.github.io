---
layout: post
title: "iOS Life Cycle 정복하기" 
date: 2021-06-08
category: read 
excerpt: ""


---

## iOS Life Cycle 정복하기

> Life Cycle이란 Life Cycle은 다 정복할 포스팅 ^___^v

이 글을 쓰게된 이유... 내 의식의 흐름 때문에...  
(목차대로 보려면 스크롤을 좀 더 내리세요)

## 의식의 흐름

* 🙍‍♀️ 기본 ViewController Life Cycle을 정리해보자...
  * 👩‍🎓 [🔗 ViewController 생명주기 정리](https://iamcho2.github.io/2021/06/02/viewcontroller-life-cycle)

* 🙍‍♀️ `viewWillAppear()` 는 너무 빠르고 `viewDidAppear()` 는 너무 느리네...  
  그 사이에 뭐 있지 않았나? 
  * 👩‍🎓  `viewDidLayoutSubViews()`가 있지!
  
* 🙍‍♀️ 오.. 그러면 속도가 딱 적당할 것 같긴 한데... `viewDidLayoutSubViews()` 오버라이딩 해서 써도 되나?
  * 👩‍🎓 됨  
  
    > viewDidLayoutSubviews is sometimes much more appropriate place to put code which specifically needs to layout view. For instance, in viewWillAppear the hierarchy is often incomplete and not ready yet. viewDidAppear is too late, as it happens after the animation. - [stackoverflow](https://stackoverflow.com/questions/39780470/viewdidlayoutsubviews-no-longer-called-when-popping-top-view-with-uinavigationco)
    >
    > Your view controller can override this method to make changes after the view lays out its subviews. The default implementation of this method does nothing. - [Apple Development Docs](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621398-viewdidlayoutsubviews)
  
* 🙍‍♀️ 그래? 오버라이딩 해서 써도 되면... 나처럼 써도 되나? 정해진 용도가 있나?

  * 👩‍🎓 [Apple Development Docs](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621398-viewdidlayoutsubviews)

* 🙍‍♀️ `viewDidLayoutSubViews()` 은 근데 왜 있는거지?

  * 👩‍🎓 [Apple Development Docs](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621398-viewdidlayoutsubviews)

* 🙍‍♀️ `viewDidLayoutSubViews()` 처럼 기본(?) ViewController Life Cycle 외 함수가 또 뭐가 있지? 그것들의 순서는?
  * 👩‍🎓 기본 ViewController Life Cycle, 비(?)기본 ViewController Life Cycle이 있는 게 아니라,  
    ViewController Life Cycle이 있고 View Life Cycle이 있는 것.  
     `updateConstraints()`, `intrinsicContentSize()`, `updateViewConstraints()` ... 등등  
    [🔗 Render Loop을 ViewController, View Life Cycle과 살펴보기](https://iamcho2.github.io/2021/06/09/view-viewcontroller-layout-cycle-with-render-loop)
  
* 🙍‍♀️ 오버라이딩은 다 되나?
  * 👩‍🎓 [정리 포스팅]()
  
* 🙍‍♀️ `setNeedsLayout()`, `layoutIfNeeded()` 는 뭐지?
  * 👩‍🎓 매 사이클에 포함되어 있는 건 아니고, View의 컨텐츠가 변경될 때 View를 다시 그려야 할 필요가 있음을 시스템에 알리는 역할.  
    이는 개발자가 따로 해줘야 하는 것 !! 다음 드로잉 사이클동안 View를 업데이트해야 함을 시스템에 알림. -> `setNeedsLayout()`
  * `layoutIfNeeded()` 은 다음 드로잉 사이클까지 기다리지 않고 즉시 !! 업데이트 함.
  
* 🙍‍♀️ UIView Life Cycle 있는 건 알겠는데... layout pass는 뭐지?
  * 👩‍🎓 업데이트되어 설정된 제약조건에따라 뷰 트리를 레이아웃
  
* 🙍‍♀️ 그거 말고 다른 pass도 있나? 있으면 뭐가 있지?
  * 👩‍🎓 오토 레이아웃 진행 과정 중, 3개의 pass를 거치는데   
    보통은 measurement pass - layout pass - display pass 순서.
  * [The Deferred Layout Pass - Apple Docs](https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/ModifyingConstraints.html)
  
* 🙍‍♀️ Layout Cycle은 뭐고 AutoLayout Cycle이 뭐지? 그냥 같은건가?
  * 👩‍🎓 그런듯?
  
* 🙍‍♀️ 그냥 애초에 오토레이아웃이 어떻게 동작하는거길래?

  * 👩‍🎓 [WWDC15 - Mysteries of Auto Layout, Part 2](https://developer.apple.com/videos/play/wwdc2015/219/)

* 🙍‍♀️ Render Loop은 또 뭐지?

  * 👩‍🎓[WWDC18 - High Performance Auto Layout](https://developer.apple.com/videos/play/wwdc2018/220/)

* 🙍‍♀️ 이왕 Life Cycle 살펴본 김에 App Life Cycle도 보자..!

  * 👩‍🎓



## 정리된 차례대로 보기

*  [ViewController 생명주기 정리](https://iamcho2.github.io/2021/06/02/viewcontroller-life-cycle)
* [Render Loop을 VC, View Life Cycle과 살펴보기](https://iamcho2.github.io/2021/06/09/view-viewcontroller-layout-cycle-with-render-loop)





### 출처

