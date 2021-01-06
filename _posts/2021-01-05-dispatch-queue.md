---
layout: post
title: "DispatchQueue" 
date: 2021-01-05
category: read 
excerpt: ""

---

# DispatchQueue

홈 뷰 section 별 gradient 배경을 지정할 때, 스크롤에 따른 셀 사라짐 이슈가 있었는데  
DispatchQueue 를 쓰니까 해당 이슈가 사라졌었다,,  이유는 나도 몰러;  

```swift
DispatchQueue.main.async {
            for index in 0 ..< tableView.visibleCells.count {
                let zPosition = CGFloat(tableView.visibleCells.count - index)
                tableView.visibleCells[index].layer.zPosition = zPosition
            }
        }
```

얘가 뭔 짓을 해줬길래.. 오류가 해결됐나...!!! 알고 싶었다,,  
그리고 지금 새로 생긴 이슈도 연관이 있을 것 같기도 하고,,

그래서 공부해보는 DispatchQueue ~!

 [Docs](https://developer.apple.com/documentation/dispatch/dispatchqueue) 왈..

```
An object that manages the execution of tasks serially or concurrently on your app's main thread or on a background thread.
앱의 메인 스레드나 백그라운드 스레드에서의 task들의 실행을 연속적 or 동시에 관리하는 객체
```

> 스레드 : 프로그램(프로세스) 실행의 단위
> 하나의 프로세스는 여러개의 스레드로 구성이 가능함.
> 하나의 프로세스를 구성하는 스레드들은 프로세스에 할당된 메모리, 자원 등을 공유
> 프로세스와 같이 실행, 준비, 대기 등의 상태를 가지며 상태가 변할 때 마다 스레드 context switching 수행

> Multi-threading : 여러 개의 스레드가 동시에 진행되는 것  
> 하나의 프로세스 내에 여러 개의 스레드가 존재하고,  
> 스레드들이 프로세스의 자원을 공유하되 실행은 독립적으로 이루어지는 구조

암튼 Swift에서 제공하는 비동기 처리방식, 멀티스레딩 처리를 위해 제공하는 API 중 하나인 GCD를 먼저 보자,,

# GCD : Grand Central Dispatch

Swift에서 Thread 작업은 GCD를 통해 처리한다.  
어느 작업을 어느 스레드가 할 건지, 멀티 스레딩 처리를 할건지, 코어를 여러 개 사용할 지,  
Sync로 할 지, 아니면 Async로 할 지 등,, 을 처리하는 것이 GCD API.

클로져로 표현된 특정 작업들을 특정 Queue에 올려 태우고, 
그 Queue를 특정 스레드에 Dispatch 출격시켜 실행하는 작업을 수행한다.  
Apple이 만든 GCD API를 활용해야 Thread-safe 하고 시스템 효율적인 멀티 스레딩 작업을 수행할 수 있다고 함

=> 주로 시간이 오래 걸리는 작업을 수행하면서 사용자의 UI를 막지 않고자 할 때 사용

# Dispatch Queue

> 클래스.  
> 실제로 해야 할 Task를 담아두면 선택된 스레드에서 실행을 해주는 역할
>
> GCD는 Tread Safe한 Dispatch Queue를 사용하여 멀티 스레딩 작업을 수행하게 해 줌.

### GCD가 제공하는 Queue들

* main (**serial**)

  * Main thread에서 처리되는 serial queue
  * 모든 UI 작업은 Main Queue에서 수행해야 함
  * 등록된 작업을 한 번에 하나씩 차례대로 처리함
  * 처리된 작업이 완료되면 다음 작업을 처리

* global (**concurrent**) `attributes: .concurrent`

  * 편의상 사용할 수 있게 만들어 놓은 Concurrent Queue
  * 전체 시스템에 공유되는 concurrent queue
  * 작업의 우선순위를 사전에 정의한 4가지 QoS를 활용하게 ..
  * 등록된 작업을 한 번에 하나씩 처리하지 않고 여러 작업을 동시에 처리

* 커스텀

  * 개발자가 임의로 정의하는 queue

  * 임의의 serial queue를 만들고 싶을 때 사용

  * 같은 serial queue인 main queue에서 처리하기엔 부적합한

    뒷단에서 serial로 돌아갈 필요가 있는 작업을 수행할 때 만들어 줌.

  * global queue에서 수행



Dispatch Queue의 종류는 Serial Dispatch Queue, Concurrent Dispatch Queue   
이렇게 두가지가 있는데, 앱 실행 시 시스템에서 기본적으로 2개의 Queue를 만들어 준다.  
그게 바로 Main Queue, Global Queue.

# Quality Of Service

어떤 작업을 멀티스레딩으로 동시에 처리하고자 할 때 global queue를 쓰게 되는데,  
우선순위는 QoS를 사용해서 지정함  
QoS는 중요도와 우선순위에 따라 User-interactive, User-intiated, Utility, Background 로 구분됨

* userInteractive
  * 중요도가 높고 즉각적인 반응이 요구되는 작업을 위해 가장 자원을 많이 쓰도록 하는 QoS
  * 유저가 누르면 즉각적으로 반응하는 작업이 들어감
  * UI업데이트, 이벤트 핸들링 등 가볍고 신속히 처리가 필요한 작업 수행용
  * global queue 항목이지만 main thread에서 실행되는 QoS (!= main queue)
* userInitiated
  * userInteractive 정도는 아니지만 유저가 빠른 결과를 기대할 때 사용
  * 유저가 실행시킨 작업으로 Async 하게 처리해야할 일들을 처리
  * 유저가 UI상 blocked되지 않게 하면서도 빠른 결과를 기대할 때 사용
* utility
  * 시간이 오래 걸리는 작업을 처리. 유저에게 처리 진행 모습도 보임
  * progress bar 같은 거..
  * 계산, I/O, 네트워킹, 데이터 피드 등의 작업에 사용
* background
  * 유저의 인지 X 뒷단에서 실행되는 작업들
  * user interaction이 필요한 것도 아니고 급하지 않은 작업들이 수행됨

# sync / async

Dispatch Queue는 sync와 async라는 메소드를 가지고 있음. 각각 동기, 비동기  

* sync
  * 동기 처리 메소드
  * 해당 작업을 처리하는 동안 다음으로 진행되지 않고 계속 머물러 있음
* async
  * 비동기 처리 메소드
  * sync와는 다르게 처리를 하라고 지시 후 다음으로 넘어가 버림





### 참고한 곳

[여기](https://m.blog.naver.com/jdub7138/220949191761)

