---
layout: post
title: "UITableView DispatchQueue" 
date: 2021-01-06
category: read 
excerpt: ""

---

# UITableView Cell Life Cycle

**cellForRowAt**, **willDisplay**, **didEndDisplaying**, **prefetchRowsAt**, **cancelPrefetchingForRowsAt**

ViewController에는 이런 함수들이 있다고 하고,

**awakeFromNib**, **prepareForReuse**, **deinit**

xib cell에는 이런 함수들이 있다고 하자.  
그리고 한 화면에는 18개의 cell이 보이는 뷰!



시뮬레이터를 켜서 실행해보면! 스크롤 하기 전에 !! 그냥 시뮬 실행만 하고 안 건드렸을 때

```
-------------- [awakeFromNib] --------------

cellForRowAt: 0
Will Display Cell : 0

-------------- [awakeFromNib] --------------

cellForRowAt: 1
Will Display Cell : 1

-------------- [awakeFromNib] --------------

cellForRowAt: 2
Will Display Cell : 2

-------------- [awakeFromNib] --------------

cellForRowAt: 3
Will Display Cell : 3

-------------- [awakeFromNib] --------------

cellForRowAt: 4
Will Display Cell : 4

-------------- [awakeFromNib] --------------

cellForRowAt: 5
Will Display Cell : 5

-------------- [awakeFromNib] --------------

cellForRowAt: 6
Will Display Cell : 6

-------------- [awakeFromNib] --------------

cellForRowAt: 7
Will Display Cell : 7

-------------- [awakeFromNib] --------------

cellForRowAt: 8
Will Display Cell : 8

-------------- [awakeFromNib] --------------

cellForRowAt: 9
Will Display Cell : 9

-------------- [awakeFromNib] --------------

cellForRowAt: 10
Will Display Cell : 10

-------------- [awakeFromNib] --------------

cellForRowAt: 11
Will Display Cell : 11

-------------- [awakeFromNib] --------------

cellForRowAt: 12
Will Display Cell : 12

-------------- [awakeFromNib] --------------

cellForRowAt: 13
Will Display Cell : 13

-------------- [awakeFromNib] --------------

cellForRowAt: 14
Will Display Cell : 14

-------------- [awakeFromNib] --------------

cellForRowAt: 15
Will Display Cell : 15

-------------- [awakeFromNib] --------------

cellForRowAt: 16
Will Display Cell : 16

-------------- [awakeFromNib] --------------

cellForRowAt: 17
Will Display Cell : 17

-------------- [awakeFromNib] --------------

cellForRowAt: 18
Will Display Cell : 18
prefetch : [[0, 19], [0, 20], [0, 21], [0, 22], [0, 23], [0, 24], [0, 25], [0, 26], [0, 27], [0, 28]]
```

요런 식으로 콘솔에 출력이 된다고 한다 !

19개의 Cell이 생성되면서 **awakeFromNib**을 호출  
**tableView(_:cellForRowAt:)**에 의해 셀들이 생성(메모리에 로드됨) 되면서 호출  
화면에 보이지기 직전에 작업을 하는 **tableView(_:willDisplay:forRowAt:)**가 호출되는 식으로  
셀 하나당 반복하게 되고 화면에 표시되는 셀의 갯수만 동작을 하게 된다!

그 후로는 **prefetchRowsAt**이 호출되는데, 화면에 표시되는 셀들 다음 셀부터 prefetching되는 것을 확인할 수 있다.  
화면에 보이는 셀 이후의 셀들을 미리 호출하는 것 !



## 순서대로 함수 알아보기

### awakeFromNib

* nib 파일이 생성된 후 초기화 작업을 준비하는 곳.
* MVC에서 ViewController를 상속받은 View가 아니라서 viewDidLoad가 없다.
* Cell에서는 View의 역할을 하기 위해 awakeFromNib()이 존재한다
* 여기서 초기화 작업을 진행한다

### **tableView(_:cellForRowAt:)**

* TableView의 특정 위치에 삽입할 Cell에 대해 Data Source에 요청하는 메서드

### **tableView(_:willDisplay:forRowAt:)**

* 테이블 뷰는 셀을 사용하여 행을 그리기 직전에 Delegate에게 이 메세지를 보냄
* 즉, Delegate가 셀 객체를 그리기 전에 사용자가 정의할 수 있음

### UITableViewDataSourcePrefetching

* 디스플레이에 보여지는 셀 이외의 셀 정보를 미리 호출하여 데이터를 받아올 수 있는 프로토콜
* 두가지 프로토콜 메서드가 존재하고, prefetchDataSource 프로퍼티도 존재한다.

![image](https://user-images.githubusercontent.com/28949235/103747452-05319380-5046-11eb-812b-e4c2c6916c17.png)

# 스크롤하면?

```
-------------- [awakeFromNib] --------------

cellForRowAt: 18
Will Display Cell : 18
prefetch : [[0, 19], [0, 20], [0, 21], [0, 22], [0, 23], [0, 24], [0, 25], [0, 26], [0, 27], [0, 28]]

-------------- [awakeFromNib] --------------

cellForRowAt: 19
Will Display Cell : 19
prefetch : [[0, 29]]
Did End Display Cell : 0

-------------- [prepareForReuse] --------------

cellForRowAt: 20
Will Display Cell : 20
prefetch : [[0, 30]]
Did End Display Cell : 1

-------------- [prepareForReuse] --------------

cellForRowAt: 21
Will Display Cell : 21
prefetch : [[0, 31]]
Did End Display Cell : 2

-------------- [prepareForReuse] --------------

cellForRowAt: 22
Will Display Cell : 22
prefetch : [[0, 32]]
Did End Display Cell : 3

-------------- [prepareForReuse] --------------

cellForRowAt: 23
Will Display Cell : 23
prefetch : [[0, 33]]
```

19번째 cellForRowAt을 보면 **tableView(_:didEndDisplaying:forRowAt:)** 이 호출됨.

### didEndDisplaying

테이블뷰로부터 cell이 화면에서 사라지면 호출되는 메서드

### prepareForReuse

20번째 셀 부터는 awakeFromNib이 아니라 prepareForReuse가 호출되는데,  
테이블뷰 delegate에 의해 재사용 가능한 셀을 준비하는 메서드임.

만약 셀이 재사용된다면 dequeueReusableCell(withIdentifier:) 메서드가 return되기 전 호출됨  
성능상의 이유로, 컨텐츠과 관련 없는 alpha, editing, selection state같은 셀의 속성만 재설정해야 함

큐에 넣어졌던 셀을 재사용하기 때문에 더 이상 셀을 생성하지 않고 재사용하는 방법을 사용하는 것!  
재사용된 셀을 사용할 땐 prepareForReuse()만 호출됨.



스크롤 맨 마지막에 닿았을 땐 지금까지 순서대로 진행됐던 로직이 반대로 진행됨



### 참고한 곳

[여기](https://jinnify.tistory.com/58) 랑 [여기](https://purple-log.tistory.com/19)