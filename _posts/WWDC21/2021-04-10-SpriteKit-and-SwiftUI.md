---
layout: post
title: "SpriteKit and SwiftUI" 
date: 2021-04-08
category: read 
excerpt: ""

---

# SpriteKit and SwiftUI

> 를 사용하기로 결정... 이제 튜토리얼 시간!

## SpriteKit을 SpriteView를 사용해서 SwiftUI에서 사용하기

> [출처](https://www.youtube.com/watch?v=4XKOPH6PXBY)

```
import SpriteKit
import SwiftUI
```

```swift
class GameScene: SKScene {
	override func didMove(to view: SKView) {
    physicsBody = SKPhysicsBody(edgeLoopFrom: frame)
  }
  
  override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
    guard let touch = touches.first else { return }
    let location = touch.location(in: self)
    
    let box = SKSpriteNode(color: .red, size: CGSize(width: 50, height: 50))
    box.position = location
    box.physicsBody = SKPhysicsBody(rectangleOF: CGSize(width: 50, height: 50))
    addChild(box) // 오브젝트 뷰에 추가
  }
}
```

요런 식으로 따로 SKScene을 만든 후에, ContentView의  body에서 보여주는 방식이다.

```swift
struct ContentView: View {
  var scene: SKScene { // scene 생성
    let scene = GameScene()
    scene.size = CGSize(width: 300, height: 400)
    scene.scaleMode = .fill
    return scene
  }
  
  var body: some View {
    SpriteView(scene: scene) // SpriteView 사용
    	.frame(width: 300, height: 400)
  }
}
```

이렇게 body에서 SpriteView를 사용해 scene을 넘겨줘서 보여주면

![image](https://user-images.githubusercontent.com/28949235/114293770-d22eee00-9ad3-11eb-805a-8c56b326b511.png)

요런 식으로 ..! (검은 화면 아무데나 클릭하면 박스가 만들어지고, 아래로 떨어져서 쌓이는 형태)

## SKScene

> [출처](https://www.youtube.com/watch?v=gNhFUE6wkto&list=PL_XkuR-7VWcsEqe6xYTwymht_GV6w9L1q&index=4)

위에서 SKScene이라는 클래스를 만들어서 뷰에 붙여줘야 한다는 내용은 이제 알겠으니까,,  
이제 SKScene에 대해서 공부해보자!

**SKScene** : 뷰 컨트롤러 위에 올라갈 화면을 만드는 클래스  

* 초기화
  * init()
  * sceneDidLoad()
  * didMove(to:)
  * 셋중하나 택1 (별 차이 크게 없음, 기본 프로젝트에서는 didMove 사용)
* 콜백함수
  * update(_:)
  * didBegin(_:)
  * touchesBegan, Moved, Ended, Cancelled(_:with:)

* scaleMode (실물기기 = 파란 사각형)
  * ![image](https://user-images.githubusercontent.com/28949235/114293967-bdebf080-9ad5-11eb-9e45-d0441279eab7.png)

> 아 그래서 위에서 .fill(비율무시)을 줬군,,~!!

* node : 화면에 보여지는 개체의 개수
* fps : 화면을 1초에 몇 번 갱신하는지
* .sks 파일 : 게임 화면 