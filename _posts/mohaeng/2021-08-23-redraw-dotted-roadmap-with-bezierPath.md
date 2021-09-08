---
layout: post
title: "BezierPath로 dotted(dashed) 곡선 로드맵 그리기" 
date: 2021-08-23
category: read 
excerpt: ""
---

# BezierPath로 dotted(dashed) 곡선 로드맵 그리기

### 바뀐 뷰

<img src="https://user-images.githubusercontent.com/28949235/130410140-80cb1679-f7b8-4308-a396-31d2dcc54ed7.png" alt="image" width=250 /><img src="https://user-images.githubusercontent.com/28949235/130410276-7b47f2b8-bae6-4905-9a73-6b71c9de8d80.png" alt="image" width=550 />

쟈니 -> 모행으로 릴리즈를 준비하면서, 내가 맡았던 roadmap 뷰가 바뀌었다.  
기존 뷰(왼) path는 그냥 실선이고, arc랑 line만을 사용해서 그리면 되어서 그리기 비교적 쉬웠다.  
근데 바뀐 뷰는 기존(색만 분기처리)과는 다르게 점선인지 실선인지 분기처리 하는 것도 추가됐다.

### 원래는 ,,

원래는 이렇게 생각했다.  
어차피 arc랑 line 그리는 건 똑같은 것 같고, 기존처럼 영역만 잘 나눠서  
실선을 dashed line으로만 바꾸면 되는 거 아니야? 점선은 어떻게 그리지?

```swift
private let beforeLine = CAShapeLayer()
let path = UIBezierPath()
// addLine,addArc 등등 하고
beforeLine.fillMode = .forwards
beforeLine.fillColor = UIColor.clear.cgColor
beforeLine.lineWidth = Size.strokeSize
beforeLine.path = path.cgPath

// 둥근 점선 구현하는 부분!
beforeLine.lineCap = .round
beforeLine.lineDashPattern = [10, 30]
        
self.contentView.layer.insertSublayer(beforeLine, at: 0)
```

기존 실선 그리는 코드에서 두 줄만 추가해주면 된다.

```swift
beforeLine.lineCap = .round
beforeLine.lineDashPattern = [10, 30]
```

선의 끝 (= 점선의 끝과 끝)을 둥글게 처리하고, DashPattern을 적용해서  
10만큼 그리고, 30만큼 쉬고, 10만큼 그리고, 30만큼 쉬고를 반복해주면 된다.

### 그럼 우선 path를 그리자!

<img src="https://user-images.githubusercontent.com/28949235/130411859-612fb728-2f1e-4f46-b116-94c441ae03e7.png" alt="image" width=600 />

그 전에 cell을 이렇게 짝수/홀수로 나눠뒀기 때문에,   
이번에도 그렇게 나눠봐야지 하고 설계를 했다.

<img src="https://user-images.githubusercontent.com/28949235/130412180-fcab68dc-85da-4efb-8eca-dd8b23334b9a.png" alt="image" width=500 />

이렇게 나누면 되겠구나 ! 하고 구현을 해봤더랬다.  
어차피 실선/점선 분기처리는 나중에 하면 되고, 실선이야 고려할 사항이 없으니  
다 점선인 경우를 먼저 구현해보고자 했다.

<img src="/Users/choyi/Library/Application Support/typora-user-images/image-20210823170447436.png" alt="image-20210823170447436" width=300px />

허거걱 너무 이뻐 ..!>!!!!!!!!

그런데 대참사가 일어났다.

### 점선 때문에,,

![image](https://user-images.githubusercontent.com/28949235/130411074-7a21a152-32de-4f05-84d9-7cba8302d373.png)

***더 이상은~ 점선 때문에~ 울고 웃지 않게 나 두발로 서 있을래***

점선이 작동되는 원리는 다음과 같았다.  
path는 내가 그리고, lineDashPattern을 내가 지정해주면  
알아서 dash pattern에 맞게 점선을 그려준다.

그래.. 내가 시키는대로 다 하는 거 좋다 이거야..

<img src="https://user-images.githubusercontent.com/28949235/130412730-c4dd3d65-af46-4cf1-a8ee-521078c12de3.png" alt="image" width=300px />

반복만을 고려해서 이렇게 cell을 나눠봤었는데, 문제점이 생겼다. ( 점선때문에..~ )

![image](https://user-images.githubusercontent.com/28949235/130413925-3195e3a9-41d9-4d98-84f3-fca4b5553916.png)

First cell에서는, 주황색 점선을 왼->오 방향으로 그린다. 이 때, 시작점은 왼쪽 끝이다.  
그 다음 Even cell에서는, 위의 사분원을 살짝 공유하는 부분이 있기 때문에  
그리긴 그려야 하는데, (리디자인 전에도 겹치는 부분이 살짝 있어서 그려줬었다) **점선인게** 문제였다.

Even cell에서는 위의 사분원만 살짝 공유하기 때문에 사분원만 살짝 그려줬더니,  
(필요한 호만 그리기 위해 각도를 지정해줘도 되지만, 각도를 쉽게쉽게 관리하기 위해 90도의 배수만 사용했다.)
아래와 같은 결과물이 나왔다.

<img src="https://user-images.githubusercontent.com/28949235/130414525-6e76e5a5-03b2-4c34-9f3a-f1c225eee5d5.png" alt="image" width=400 />

그랬더니... 점선의 시작점이 서로 달라서 **점선 패턴이 겹쳐지지 않았다**.  
위 그림에서 보라색으로 표시된 것 처럼, First cell에서 그렸던 것을 그대로 모두 그려줘도 되겠지만,  
실제로 보이지도 않는 선을 그려주는 게 비효율적이라고 느껴졌다.

<img src="https://user-images.githubusercontent.com/28949235/130416156-e3815820-ef8e-449b-96a3-e1269f3c6d82.png" alt="image" width=600px />

그래서 셀을 다르게 분리해 설계했다.  
그리는 건 어차피 똑같이 그리는데, (위의 보라색 선을 그리는 것과 동일)  
이렇게 분리하는 게 그 전처럼 애매~하게 겹치게 분리하는 것 보다 더 직관적이라  
유지보수 하기도 쉽고, 다른 사람이 이해하기도 더 쉽다고 생각했기 때문이다.

또한, 이렇게 바꾸는 김에,  
기존엔 Cell에서 각각 Size라는 Enum을 만들어 NameSpace를 관리했었는데,  
이번엔 세가지 cell에서 공통으로 접근할 수 있는 Enum을 만들고,  
그 내부에 공용으로 사용 가능한 CGPoint들을 만들어서  
세가지 Cell에서 각각 따로 변수들을 만들지 않고 공용으로 사용할 수 있게 리팩토링 했다.

**겹치는 두 가지 path를 그리는 함수**도 만들어서 아예 함수를 호출해서 쓰면 더 좋을 것 같다는 생각도 들었다!

<img src="https://user-images.githubusercontent.com/28949235/130441320-5faf1c22-473d-408b-9dbc-72b30e444ee1.png" alt="image" width=500 />

이렇게... path1과 path2를 그리는 함수를 만들어 놓으면 모든 cell에서 사용할 수 있기 때문이다.

![image](https://user-images.githubusercontent.com/28949235/130445649-c09b7239-59c8-4b29-af18-db45e0e67f4e.png)

아래 핑크색 path가 함수를 따로 만들어서 그린 path다.

![image](https://user-images.githubusercontent.com/28949235/130445762-894cadb7-93e8-4dcc-85a8-94e866da64a8.png)

근데 또 반원끼리 만나는 지점에서 (path 1, path 2가 만나는 지점) 시작점 문제가 생겨서  

<img src="https://user-images.githubusercontent.com/28949235/130451468-229d5ea7-fb6f-4fa8-8278-812f31771909.png" alt="image" width=200px />

(파란 선을 보면) 저렇게 원 가운데에 맞게 점선 한개가 위치해야 하는데,  

![image](https://user-images.githubusercontent.com/28949235/130451630-6d65bd80-f981-4fb5-ac9d-93e9d0e72a18.png) 

점선이 이렇게 위치하는 문제가 생겼다.

그래서 결국엔.. 그냥 Even에서 Upward, Downward / Odd에서 Upward, Downward path를 따로 그려줬다.  
그리는 시작점, 즉 그려가는 방향만 다르기 때문에 만들어둔 CGPoint들은 재활용할 수 있었다.

저것도 dot들이 안 맞아서...

결국엔 중간부터 그려주는 방식을 택했다 ㅠ_ㅠ
