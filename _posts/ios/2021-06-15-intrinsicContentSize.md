---
layout: post
title: "intrinsicContentSize" 
date: 2021-06-15
category: read 
excerpt: ""

---

# intrinsicContentSize

[목차](https://iamcho2.github.io/2021/06/08/master-of-ios-life-cycle)



## 서론

VC + View Life Cycle이

![](https://user-images.githubusercontent.com/28949235/121391424-504f2a00-c989-11eb-8566-9142018c66a1.png)

이라고 굳게 믿고 있었던 나,,, `intrinsicContentSize()` 에 대해 공부하려다가 이상한 점을 발견하는데,,,  
프로젝트 만들어서 오버라이드 하려고 보니 안 되길래 뭔가 했더니 함수에서 변수로 바뀌었습니다..~ (대충 17년도 쯤)

### 원래는

```swift
override public func intrinsicContentSize() -> CGSize
{
   ...
}
```

요런식으로 CGSize를 반환하는 함수였지만,  

### 지금은

```swift
override open var intrinsicContentSize: CGSize {
    //...
    return myCGSize
}
```

요런식으로 사용하면 된다.

###  애초에 intrinsicContentSize가 뭔데?

[Apple Docs](https://developer.apple.com/documentation/uikit/uiview/1622600-intrinsiccontentsize)

intrinsic: 본질적인!  
뷰 자신의 프로퍼티만 고려한 뷰의 본질적인 크기를 의미한다.

커스텀 뷰가 나타내는 컨텐츠들은 보통 레이아웃 시스템이 인지하지 못하는데,  
`intrinsicContentSize` 프로퍼티를 설정해주면 커스텀 뷰가   
컨텐츠들에 따라 원하는 크기를 레이아웃 시스템에게 전달할 수 있게 해준다.  
`intrinsizContentSize`는 컨텐츠의 frame과는 독립적이여야 하는데,  
예를 들어 레이아웃 시스템에게 변경된 높이에 따른 변경된 넓이를 동적으로 전달할 수 있는 방법이 없기 때문!

### 쉽게 이해하기

![image](https://user-images.githubusercontent.com/28949235/122041749-3e9bd580-ce14-11eb-82d2-984629c6ba12.png)

UILabel 같은 경우는 intrinsicContentSize를 가지고 있어서,  
width와 height를 auto layout으로 잡아주지 않아도 오류(빨간줄)가 나지 않는다!

그리고 저 상태에서 런타임 때 label의 내용을 바꿔줘도, text가 "Label" 일 때의 크기 그대로가 아니라  
바꾼 text의 크기 만큼 UILable 크기가 변한다. (텍스트가 잘리지 않음)  
auto layout도 잘 잡혀있다.

> height, width에다 constraint를 줘버리면 크기가 고정되므로 변하지 않음!

이게 왜 되는거냐면, UILabel의 intrinsicContentSize가 컨텐츠의 크기를 알아서 계산해주기 때문!  
view의 컨텐츠 크기가 바뀌었을때 intrinsicContentSize 프로퍼티를  통해 size를 갱신하고  
그에 맞게 auto layout이 업데이트되도록 해 주는 것이다 ~!!

![image](https://user-images.githubusercontent.com/28949235/122042351-ff21b900-ce14-11eb-9649-0d57eadcfd62.png)

반면에, UILabel이 아니라 UIView로 아까와 같이 top, leading constraint를 잡아주면 빨간줄이 뜬다.  
UIView는 height, width 둘 다 intrinsicContentSize가 없기 때문!

이 외에도, intrinsicContentSize를 height이나 width 둘 중 하나만 가지고 있는 애들도 있고,  
내용에 따라 intrinsicContentSize 유무가 바뀌는 애들도 있다.

예를 들어, UIImageView는 이미지가 없을 땐 intrinsicContentSize가 없다가   
이미지를 지정해주면 intrinsicContentSize가 생긴다.

### 언제 호출되는데?

- intrinsicContentSize를 참조하는 순간은 view가 updateConstraints를 호출한 직후 한번
- 이 view 는 UIlabel의 text가 변하는것처럼 내부 사이즈가 변할때마다 invalidateIntrinsicContentSize호출

### invalidateIntrinsicContentSize?

```swift
open override func invalidateIntrinsicContentSize() {
    super.invalidateIntrinsicContentSize()
}
```

아까 위에서 본 애들은 시스템에서 자동으로  
view의 컨텐츠 크기가 바뀌었을때 intrinsicContentSize 프로퍼티를 참조하고, size를 갱신하고,  
그에 맞게 auto layout 업데이트도 해 주는데, 

이게 custom view라면 얘기가 달라진다.  
custom view인 경우에는 `intrinsicContentSize`, `invalidateIntrinsicContentSize()`를 직접 구현해줘야 한다.

만약에 구현을 안 해주면 무슨 일이 생기냐면 !!  
위의 UILabel처럼 내용을 변경했다고 자동으로 그 컨텐츠에 맞게 크기가 늘어나는 것이 아니라,  
초기 크기 그대로 고정되어 버린다. 내용이 잘리든지 말든지~

### 더 알아보기

IntrinsicContentSize에서 더 나아가  
Hugging Priority, Compression Resistance Priority 를 알아보고 싶다면  
[여기](https://babbab2.tistory.com/135)에 잘 나와있음!

### 추가로,,,

[김종권의 iOS 앱 개발 알아가기](https://ios-development.tistory.com/233) 에서  
버튼의 크기를 width, height로 지정하는 것 보다 intrinsicContentSize를 바꾸는 것이 더 좋은 방법이라고 한다!

![image](https://user-images.githubusercontent.com/28949235/122043928-de5a6300-ce16-11eb-9fce-cb5261805705.png)

여기서 지정가능!

![image](https://user-images.githubusercontent.com/28949235/122043957-e6b29e00-ce16-11eb-930e-5d1cc4f90865.png)

ㅎㄷㄷ~ 커스텀 버튼인 경우에도 font와 size등의 작업을 더욱 유연하게 할 수 있어서  
intrinsicContentSize 로 작업하는 게 더 좋다고 한다~!

### 출처

[SOF](https://stackoverflow.com/questions/38881151/intrinsiccontentsize-method-does-not-override-any-method-from-its-superclass) [마기의 개발 블로그](https://magi82.github.io/ios-intrinsicContentSize/) [김종권의 iOS 앱 개발 알아가기](https://ios-development.tistory.com/233)
