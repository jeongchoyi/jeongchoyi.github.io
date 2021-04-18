---
layout: post
title: "SpriteKit 이해하기" 
date: 2021-04-12
category: read 
excerpt: ""

---

# SpriteKit 이해하기

> [출처](https://www.youtube.com/watch?v=JaehPv89x3U&list=PL23Revp-82LIKiBNSJaF311ixNvSzk8LC&index=2)

### Sprite가 뭔데?

Sprite는 SKNode의 서브클래스인 SKSpriteNode 객체!  
서브클래스는 여러개가 있는데, SKLabelNode, SKVideoNode, SKShapeNode ...

다른 객체와 같이, Sprite도 메서드와 프로퍼티가 존재한다.  
position, zRotation, xScale, yScale, alpha, hidden, -addChild: ,...  와 같은 것들은  
부모클래스인 SKNode에서 상속받은 것들이고,  
size, texture, color, ... 같은 것들은 SKSpriteNode 고유의 것!

### Sprite 만들기

```swift
let background = SKSpriteNode(imageNamed: "background1")
```

**위치 지정하기** - position, anchor point, z position property 사용

* position : X, Y 좌표
* anchor point : X, Y 좌표 기준 (0~1)
* z position : Z 좌표

```swift
background.position = CGPoint(x: size.width/2, y: size.height/2)
background.anchorPoint = CGPoint(x:0.5, y:0.5) //default
background.zPosition = -1
addChild(background)
```

position 과 anchorPoint 요 두줄로 가운데 정렬 시키는 것

**회전시키기**

```swift
background.zRotation = CGFloat(M_PI) / 8
```

## Actions

회전, 크기조절, 시간에 따른 sprite의 위치 조절 등을 할 수 있다!  
Action을 다루는 클래스 이름은 SKAction

SKAction에는 다양한 메소드들이 있는데, 

**Move 관련**

```
moveByX:y:duration
moveBy:duration:
moveTo:duration: // 지정된 시간 동안 특정 좌표로 이동
moveToX:duration:
moveToY:duration:
followPath:duration:
followPath:speed: // 특정 path를 특정 speed로 따라가기
followPath:asOffset:orientToPath:duration:
followPath:asOffset:orientToPath:speed:
```

**scale 관련**

```
scaleBy:duration:
scaleTo:duration:
scaleXBy:y:duration:
scaleXTo:y:duration:
scaleXTo:duration:
scaleYTo:duration:
```

**rotate 관련**

```
rotateByAngle:duration:
rotateToAngle:duration:
rotateToAngle:duration:shortesUnitArc:
```

### sequence로 연결해서 사용하기

```swift
let actionMidMove = SKAction.moveTo(
	어쩌구
)

let actionMove = SKAction.moveTo (
	어쩌구	
)

let sequence = SKAction.sequence([actionMidMove, actionMove])

enemy.runAction(sequence)
```

### run block 사용하기

```swift
let logMessage = SKAction.runBlock() {
  print("Reached bottom!")
}

let sequence = SKAction.sequence(
	[actionMidMove, logMessage, wait, actionMove]
)
```

runBlock()을 호출하고 실행할 코드 블럭을 넘겨주는 action을  
sequence에 넘겨주면 특정 동작 이후에 로그를 띄우거나 할 수 있음.

### sprite 지우기 action

```swift
let actionRemove = SKAction.removeFromParent()
```

sequence 맨 마지막에 넣는다던지 하는 식으로 사용

### repeatActionForever

```swift
runAction(SKAction.repeatActionForever(
	SKAction.sequence(
    [SKAction.runBlock(spawnEnemy), 
    SKAction.waitForDuration(2.0)]
  )
))
```

계속 반복~~ 계속~~~~~~~



## Animation

### Textures

애니메이션의 각 프레임이 될 이미지 리스트들을 Textures라고 한다.  
애니메이션은 시간에 따라 texture를 자동으로 swap해주는 것

texture의 배열만 만들어주면 된다.

```swift
var textures: [SKTexture] = []
for i in 1...4 {
  textures.append(SKTexture(imageNamed: "zombie\(i)"))
}
textures.append(textures[2]) // 마지막에 두개의 텍스쳐 추가
textures.append(textures[1])

zombieAnimation = SKAction.animateWithTextures(textures, timePerFrame: 0.1)
```

초기화함수에

```swift
addChild(zombie) //이거 이후에

zombie.runAction(SKAction.repeatActionForever(zombieAnimation))
```

## Scenes

Scene들은 SKScene 클래스를 사용해서 만든다.  
얘도 물론 메서드와 프로퍼티가 있다!

```
size
scaleMode
-update:
-didEvaluateActions
-didSimulatePhysics
-didApplyConstraints
-didFinishUpdate
```

### 화면 전환하기

화면 전환할 땐 SKTransition 객체를 만든다.  
한 scene에서 다른 scene으로 갈 때 animated된 화면 전환을 수행한다.  
수행 할 수 있는 animation에는 여러가지가 있고, 관련 내용은 Docs 참고!

```swift
let reveal = SKTransition.flipHorizontalWithDuration(0.5)
self.view?.presentScene(myScene, transition: reveal)
```

   

## Labels

SKLableNode 클래스 사용  
SKLabelNode 클래스는 SKSpriteNode와는 달리 anchor point 프로퍼티가 없다.















