---
layout: post
title: "boarding pass scene 그리기" 
date: 2021-04-12
category: read 
excerpt: ""

---

# boarding pass scene 그리기

> 진짜 진짜 개발 시작!

### Scene 1

요정(ㅋㅋ)이 날아와 WINTER LAND로 여행 갈 수 있도록 하는 boarding pass를 준다.  
캐릭터(나)에게 날개가 달리는 내용..!

비행기를 태울까 하다가 환경을 생각하자는 앱인데 ㅋㅋㅋ 비행기 태우는건 모순이라고 생각해서  
날개를 부여하는 방식으로...~

**코딩 교육적 요소** String  
boarding pass에 이름칸이 비어있고, string 변수를 바꿔서  
실시간 또는 `run my code`를 눌렀을 때 boarding pass에 넘겨준 String값이 들어간 뷰를 확인할 수 있다.



### 대충 그려본 뷰

요 뷰를 짜는 시간 !!!

<img src="https://user-images.githubusercontent.com/28949235/114506921-64b6c500-9c6d-11eb-9e8b-bc9ac996423a.png" alt="제목_없는_아트워크 2" style="zoom:20%;" />

대충... 이런 뷰다.. 배경이랑 뭐랑 다 그려야되겠지만 !

미리 설계를 좀 해보자면

* 배경이랑 (요정, boardingPass) 뷰는 ZStack으로 쌓는다
* 요정은 위아래로 움직이는 animation이 적용되어야 하므로 SpriteView로 보여준다
* BoardingPass는 따로 View를 만들어서 ContentView에 붙인다
  * 이럴경우 @State 어떻게 할건지?
  * 값이 변할때마다 캐치할 수 있는 옵저버같은 존재가 필요하다.
* 비행기 심볼은 SF Symbol



### Fairy Scene 그리기

**ContentView.swift**에서

```swift
struct ContentView: View {
    var fairy: SKScene {
        let fairy = FairyScene()
        fairy.size = CGSize(width: 200, height: 300)
        fairy.scaleMode = .fill
        return fairy
    }
    
    var body: some View {
        ZStack {
            Image("bg01")
                .resizable()
            
            VStack {
                SpriteView(scene: fairy)
                    .frame(width: 200, height: 300)
            }
        }
        .edgesIgnoringSafeArea(.all)
        .aspectRatio(contentMode: .fill)
    }
}
```

SKScene을 만들고, body 부분에서 ZStack으로 배경과 fairy를 쌓아준 모습!  
이제 새 파일을 만들어서 FairyScene을 그리러 가봅시다

**FairyScene.swift**

```swift
let fairy = SKSpriteNode(imageNamed: "fairy")
```

요렇게 이미지로 SKSpriteNode를 만들어준다.

```swift
addChild(fairy)
```

화면에 보여주려면 addChild를 하면 된다.

