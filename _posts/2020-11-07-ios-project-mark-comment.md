---
layout: post
title: "iOS 프로젝트 Mark 주석" 
date: 2020-11-7
category: read 
excerpt: ""

---

# iOS 프로젝트 MARK 주석

## MARK 주석?

이렇게 말하니까 뭔가 어색해 보이지만 사실 Mark 주석은  

ViewController 파일을 만들 때 마다 보는 주석이다! 사실 이름도 MARK 주석 아닌데 내가 그냥 갖다 붙인 거임 ㅋ-ㅋ

![image](https://user-images.githubusercontent.com/28949235/98552865-517c8380-22e2-11eb-823d-6d3ba3cb1594.png)

가운데쯤 위치한 **// MARK: - Navigation** 이넘...

오늘은 이넘을 쓰는 방법을 잘 정리해보려고 한다

## 왜 .. MARK를 .. ?

걍 `// 어쩌구저쩌구` 라는 편한 주석이 있는데 이걸 왜 쓰나요?

한 파일 내에서 관련있는 부분끼리 구역을 묶어주는데, 

여러가지 이유가 있겠지만 내가 MARK 주석을 쓰려는 가장 큰 이유는

<img src="https://user-images.githubusercontent.com/28949235/98553947-85a47400-22e3-11eb-9614-1d13fb01b990.png" alt="image" width=300px />

크으... 바로 이거 때문인데, 솝트 22기 iOS파트 처음 들어왔을때인가  

저거 보고 속으로 진짜 우와 했던 기억이 있는데... 이제서야 제대로 정리를 함,, 해보자구,,~

## 쓰는 법

1. **//** 를 적는다
2. 한 칸 띄고  **MARK:** 한 칸 띄고 **- (기호)** 
   1. 콜론(:)을 붙여써야 함
   2. 기호는 생략 가능 - 생략하면 구조 화면에서 구분선이 없음

3. 한 칸 띄고 **표시하고 싶은 구역 명**을 적는다

```
// MARK: - Properties
```



## 그럼 어떤 구역을 구분해야 하지?

이건 자유지만, 그냥 내가 이곳저곳 찾아보면서 눈치보면서 대충 이렇게 하나부다 ~ 하고 정리한 것

**// MARK: - Properties**

**// MARK: - @IBOutlet Properties**

**// MARK: - @IBAction Properties** 			Outlet이 있으면 Action도 있지 않을까

**// MARK: - UITableViewDataSource** 		프로토콜들

**// MARK: - View Life Cycle**							viewDidLoad(), viewWillAppear(_:) ...



이런것도 있다



**// MARK: - Outlets**

**// MARK: - Variables**

**// MARK: - Actions**

**// MARK: - Overrides**

**// MARK: - Implementation**

**// MARK: - TextFieldDelegate**



이런것도



**// MARK: - IBOutlets**

**// MARK: - Class Properties**

**// MARK: -  Init & dealloc methods**

**// MARK: - UIViewController events**			viewDidLoad()...



## TODO: 와 FIXME:

**// 어쩌구:  -** 형식은 MARK만 있는게 아니다!

TODO, FIXME도 있다! 옛날엔 !!!랑 ???도 있었던 듯

**// TODO: -  어쩌구 구현하기**

**// FIXME: -  저쩌구 고치기** 

이런 식으로 똑같이 쓰면 된다. FIXME는 밴드 모양 아이콘이 뜬다 ~!

![image](https://user-images.githubusercontent.com/28949235/98556611-94d8f100-22e6-11eb-81a5-c5b1917e5ed1.png)

아우 넘 이뻐 당연히 클릭하면 이동도 되는 멋진 넘이다.. 잘 써먹어야지



**//MARK: - 포스팅 끝!**