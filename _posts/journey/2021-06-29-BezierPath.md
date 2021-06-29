---
layout: post
title: "UIBezierPath로 Course Cell 그리기" 
date: 2021-06-29
category: read 
excerpt: ""

---

# UIBezierPath로 Course Cell 그리기

![제목_없는_아트워크](https://user-images.githubusercontent.com/28949235/123799797-8a29a580-d923-11eb-857c-e019df114433.png)

저런 도형을 그려야 해서... 공부한 BezierPath

### CAShapeLayer 만들기

나중에 뷰에 붙여주기 위해서 CAShapeLayer를 만든다.

```swift
let line = CAShapeLayer()
```

### UIBezierPath 만들어서 path 그리기

```swift
let path = UIBezierPath()
```

이렇게 UIBeizerPath를 만들어 주고

```swift
path.move(to: startPoint)
```

시작점을 지정해준다. (startPoint는 CGPoint형으로 따로 변수 만들어 둠)

```swift
path.addArc(withCenter: startPoint, radius: 75, startAngle: 0, endAngle: CGFloat.pi / 2, clockwise: true)
```

quarter circle은 addArc 써서 그려주면 되는데,  
swift에서 각도가 평소 알던 각도랑 좀 다르다. 내가 아는 180도가 0도다.

![image](https://user-images.githubusercontent.com/28949235/123848619-e526c100-d952-11eb-9e7d-76b687e50f4d.png)

요렇게...

```swift
path.addLine(to: endPoint)
```

그리고 가운데 선을 그려주고

```swift
path.addArc(withCenter: endPoint2, radius: 75, startAngle: -CGFloat.pi / 2, endAngle: -CGFloat.pi, clockwise: false)
```

남은 quater circle도 그려준다.

### CAShapeLayer Property 설정

path 그렸다고 다 되는 게 아니고, 화면에 나타내주기 위한 설정이 필요하다.

```swift
line.fillMode = .forwards
line.fillColor = UIColor.clear.cgColor
line.strokeColor = UIColor.Pink.cgColor
line.lineWidth = 20.0
```

요런거 설정해주고

```swift
line.path = path.cgPath
```

path를 우리가 만든 UIBezierPath로 !

```swift
cellBgView.layer.insertSublayer(line, at: 0)
```

마지막으로 내 뷰에다가 맨 뒤에 sublayer로 넣어주면 된다.  
우리가 넣어줄 게 CAShapeLayer라서, subview가 아니라 subLayer로 넣어줘야 함!

