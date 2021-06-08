---
layout: post
title: "ViewController 화면 전환" 
date: 2021-06-03
category: read 
excerpt: ""

---

## @IBOutlet과 @IBAction

> Interface Builder Annotation 이라고 부르기도 함

Outlet은 콘센트 개념, Action은 동작 개념

* @IBOutlet
  * 처리 결과를 View에 알리고 원하는 동작을 이끔
  * View에 존재하는 요소와 Controller를 연결하기 위한 변수 개념
* @IBAction
  * 유저의 특정 이벤트 (터치, 드래그, 편집 등)을 감지해서 Controller에게 알림
  * 특정 이벤트 발생 시 실행될 동작들을 정의

## 화면 전환

**모달 Modal**

* 특정 위치를 누르면 새로운 창이 기존 창 위에 뜨는 것
* 사용자의 이목을 끌기 위해 사용
* 화면을 전환한다는 느낌보다는,  
  사용자들을 집중시켜야 하는 화면을 다른 화면 위로 띄워 표현하는 방식
* Alert, Action Sheet, Activity Views(Share Sheets), Modal View

**네비게이션 Navigation**

* 특정 버튼을 누르면 화면이 오른쪽으로 넘어가는 것

### Segue를 통한 화면 전환

> 스토리보드를 통해 출발지와 목적지를 직접 이어주는 방식

* Show
  * Navigation Controller가 연결되어 있다면 Push 처리  
    아닌 경우 Present Modally 처리
* Show Detail
  * 아이패드에서 사용하는 방식
  * Push가 아닌 Replace 방식을 사용
* Present Modally
  * Present 방식 전환
  * Modal Style, Transition Style을 변경해서 다양한 방식 사용 가능
* Popover Presentation
  * 아이패드에서 사용되는 전환 방식
  * 작은 Popup 형태의 뷰를 띄움

### 코드를 통한 화면 전환 - Modality

```swift
guard let nextVC = self.storyboard?.instantiateViewController(identifier: "SecondViewController") as? SecondViewController else { return }

self.present(nextVC, animated: true, completion: nil)
```

**style 변경하기**

```swift
(뷰컨인스턴스).modalPresentationStyle
```

* `.automatic` : 시스템에서 정한 기본 옵션
* `.fullScreen` : 위를 남기지 않고 새로운 모달로 창을 덮음
* `.overCurrentContext` : 새로운 모달창이 투명한 경우, 아래에 깔려있는 이전 뷰도 함께 보임

```swift
(뷰컨인스턴스).modalTransitionStyle
```

* `.coverVertical` : 아래에서 나와 뷰를 올리는 형식
* `.crossDissolve` : 뷰가 교차되며 스르륵 전환됨



```swift
self.dismiss(animated: true, completion: nil)
```



### 코드를 통한 화면 전환 - Navigation

> drill down interface, 깊이와 흐름을 나타내기 위해 사용하는 구조

**네비게이션의 3가지 주요 스타일**

<img src="https://user-images.githubusercontent.com/28949235/120158981-ac021080-c22f-11eb-8c90-a7d4737d3f36.png" alt="image" style="width:200px" />

* 계층 네비게이션
  * 각 화면마다 다른 화면으로 갈 수 있는 선택지를 1개 골라야 함
  * 선택지별로 계층 구조가 존재
  * ex: 설정, 메일

<img src="https://user-images.githubusercontent.com/28949235/120159088-c9cf7580-c22f-11eb-9da2-3338d93676e1.png" alt="image" style="width:200px" />

* 플랫 네비게이션
  * 큰 카테고리 내에서 화면 전환
  * 주로 탭바를 사용하는 형태
  * ex: 앱스토어, 음악

<img src="https://user-images.githubusercontent.com/28949235/120159187-df449f80-c22f-11eb-8a7d-911551f8cd8b.png" alt="image" style="width:300px" />

* 경험적 중심 또는 경험 중심 네비게이션
  * 몰입감을 중요시하는 서비스나 게임에서 많이 사용



계층 / 플랫을 섞어서 혼합 네비게이션 사용도 가능

**Navigation Controller**

> 뷰 컨트롤러 사이 계층 구조를 탐색할 수 있게 해주는 객체  
> 자식 뷰를 Navigation Stack에 쌓고 관리하는 방식

네비게이션 자체가 화면에 대한 관리를 담당하기 때문에,  
네비게이션 컨트롤러는 화면을 나타내주는 뷰 컨트롤러를 관리하는 역할을 하게 됨

가장 먼저 스택에 추가된 뷰컨이 Root View Controller가 됨  
이후에 쌓인 뷰컨들은 아래에서부터 차곡차곡 쌓임

pop : 가장 위에 있던 뷰 컨트롤러가 빠짐  
push : 가장 최근 뷰컨이 최상단에 쌓이게 됨

```swift
self.navigationController?.pushViewController(nextVC, animated: true)
```

```swift
self.navigationController?.popViewController(animated: true)
```



> SOPT 28기 iOS 파트 1차 세미나 내용 中

