---
layout: post
title: "gradient 기초 실습" 
date: 2020-12-29
category: read 
excerpt: ""

---

# Gradient

![image](https://user-images.githubusercontent.com/28949235/103268926-d5580f80-49f7-11eb-8034-31c874cbc84e.png)

나 이번에 아무래도 gradient 정복할 듯.. >.<

# CAGradientLayer

![image](https://user-images.githubusercontent.com/28949235/103276655-79e34d00-4a0a-11eb-8711-2ead1a94477b.png)

> 프로퍼티들

Gradient를 구현하기 위해서는 **CAGradientLayer 객체**를 사용한다. [Docs](https://developer.apple.com/documentation/quartzcore/cagradientlayer)  
(Core Graphics framework를 사용하는 방법도 있는데,, 기초 지식이 필요한데 난 없어,,)  
background color위에 color gradient를 그려서 layer의 shape을 채우는 **layer** 이다!  
모든 뷰 객체가 포함하고 있는 `CALayer` 클래스의 subclass이다. 

참고- 원형 gradient는 지원하지 않음! 지금은 난 필요 없지만

### 쓰는 방법 요약

>  View에 CAGradientLayer 객체를 만들어 뷰 레이어에 붙여주면(`addSublayer`) 된다.

해야 할 일  
1 `CAGradientLayer` 객체 초기화하기 (위에서 만든 `gradientLayer`)  
2 gradient layer의 frame 설정하기  
3 색상 설정하기  
4 target view의 layer에 gradient layer를 sublayer로 추가하기

## Gradient Layer 만들기

```swift
var gradientLayer: CAGradientLayer!
```

> 왜 ... force unwapping 을 .. 해야... 하지?...
> : 안 하면 오류난다. 초기값이 없어서.. 그래도 먼가 찝찝하니까 난 `?` 쓸래 ![image](https://user-images.githubusercontent.com/28949235/103270843-384ba580-49fc-11eb-85e1-847864623c7f.png)

그리고 `ViewController`에 초기화, 초기 값 설정을 위한 함수 `createGradientLayer()`를 만들어준다.  
뷰에 붙이는 거 까지 다 해주는 함수.. 이게 젤 간단한 방법. `viewDidLoad()` 나 `viewWillAppear()`에서 호출해주면 된다.

> 물론 함수 안 만들고 한줄한줄 써도 됨

```swift
func createGradientLayer() {
        gradientLayer = CAGradientLayer()
        gradientLayer.frame = self.view.bounds
        gradientLayer.colors = [UIColor.red.cgColor, UIColor.yellow.cgColor]
        self.view.layer.addSublayer(gradientLayer) 
  				//view 대신 내가 만든 UIView IBOutlet 이름 넣어도 됨
}
```

## 색상 조정

`colors` 프로퍼티 수정

```swift
gradientLayer.colors = [UIColor.red.cgColor, UIColor.yellow.cgColor]
```

위의 함수에서 요 부분에 배열로 색을 더더더 넣어주면 된다.  
주의해야 할 점은 `UIColor` 객체 말고 `cgColor` 객체여야 함!
color set 만들어서 animate도 가능하다!!! [ㄷㄷ](https://www.appcoda.com/cagradientlayer/)  
animate 쓰는것도 좋아 보이는데? 우선 tableview 적용해보고 결정해야지

## 색상 위치

>  각 색상이 얼만큼 차지하는지 관련

기본값은 색상 개수에 따라 n등분. 그리고 요건 `locations` 이라는 프로퍼티를 사용해서 수정이 가능하다.  
`NSNumber` 객체의 배열을 넘겨주면 됨. **시작 지점**을 지정해주면 되고,  
배열 인덱스가 늘어날수록 숫자는 늘어나는 형태여야 한다! (당연)  
범위는 0.0부터 1.0

``` swift
gradientLayer.locations = [0.0, 0.35]
```

> 이런 식으로 하면 두번째 색상이 `1.0 - 0.35`, 즉 65%를 차지하게 되는 것

주의!

`locations` 프로퍼티의 초기값은 `nil` 이다 주의주의 앱 터져요



