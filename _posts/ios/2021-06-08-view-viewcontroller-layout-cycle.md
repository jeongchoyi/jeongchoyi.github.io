---
layout: post
title: "View Layout 함수들 View Life Cycle과 살펴보기" 
date: 2021-06-08
category: read 
excerpt: ""

---

## View Layout 함수들 View Life Cycle과 살펴보기

> `viewDidLayoutSubviews()` 를 오버라이딩 해서 구현해야 할 일이 생겼는데,  
> 이렇게 그냥 써도 되는 것인가!? 라는 생각이 들어서 찾아본 내용 정리

MOMO를 개발할 때 해찌랑 [DispatchQueue 관련 실험](https://iamcho2.github.io/2021/01/06/dispatch-queue-with-haejji) 을 하면서  
**viewWillAppear**, **viewDidAppear** 사이  
**viewWillLayoutSubViews**, **viewDidLayoutSubViews** 요 두 놈이 있다는 것은 알게 되었지만,  
그리고 `DispatchQueue.main.async `가 그 후에 작업을 하는 것도 알게 되었지만 !!

`viewDidLayoutSubviews()` 요놈은 View Life Cycle 이랑 다른 점은 무엇인지,  
`updateConstraints()` 이런건 뭔지!!!  
그리고 오버라이딩 해서 써도 되는 것인지 (!!)를 알아보려고 한다.

## ViewController Life Cycle, View Life Cycle?

둘은 다른 것 !!  
당연 뷰 컨트롤러 위에 언제나(?) 뷰가 올라가 있으니까 헷갈렸던 것이다... 구분해서 생각해야 편한 것 같다

이 외에도 app life cycle도 있고... life cycle이라고 다 같은 life cycle이 아니다...!





## View Life Cycle 포함 전체적인 순서

> 이를 **레이아웃 사이클(또는 오토레이아웃 사이클)**이라고 하는 듯.. 

함수 호출의 순서를 보기에 앞서, 레이아웃 사이클의 3단계를 먼저 살펴보자

1. 제약 조건 업데이트 (Constraints Update)
2. 레이아웃 업데이트 (Layout Update)
3. 렌더링 (Render) (= 그리기 (Draw))





