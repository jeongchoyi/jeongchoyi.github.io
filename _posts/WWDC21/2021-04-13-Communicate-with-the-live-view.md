---
layout: post
title: "Communicate with the live view" 
date: 2021-04-13
category: read 
excerpt: ""

---

# PlaygroundLiveViewMessageHandler

> [출처](https://developer.apple.com/videos/play/wwdc2018/413#)
>
> 피폐해진 멘탈 다시 붙잡고... 고고../.....

SwiftUI로 데이터 전달을 시도하기에 앞서...  
애초에 playground page에서 데이터를 어떻게 전달하는지 살펴보려고 한다.

![image](https://user-images.githubusercontent.com/28949235/114734638-5d7fdc00-9d7f-11eb-80b1-1abe47d205ce.png)

live view에 PlaygroundLiveViewMessageHandler 프로토콜의 send 메서드를 사용해 값을 전달할 수 있다.  
(PlaygroundSupport framework 내에 선언되어 있는 프로토콜)

값은 PlaygroundValue로써 전달된다.  
PlaygroundValue도 PlaygroundSupport framework 내에 선언되어있는 enum이다.

![image](https://user-images.githubusercontent.com/28949235/114735300-fe6e9700-9d7f-11eb-8224-ca1497a5f5e5.png)

객체를 PlaygroundValue로 간단하게 변환하기 위해서,  
 `LiveViewSupport.swift`에 PlaygroundValueConterible 프로토콜을 호출한다.

그리고 이 파일을 작성한 모든 playground book의 bool-level sources folder에 포함시킨다.

그 후에, live view로 보내고 싶은 모든 타입들에 대해,  
그 타입들을 PlaygroundValueConvertible 프로토콜을 따르게 하고  
asPlaygroundValue 함수를 구현한다.  
이 함수가 원래 객체를 PlaygroundValue로 변환해 줄 것이다.

 ![image](https://user-images.githubusercontent.com/28949235/114735663-61602e00-9d80-11eb-88db-f15380bbb5b1.png)

그리고 helper function도 작성하는데, sendValue라는 함수이다.   
이 함수는 PlaygroundValueConvertible 값을 받아서  
현재 페이지의 라이브 뷰에다가 값을 PlaygroundValue로써 보낸다.

![image](https://user-images.githubusercontent.com/28949235/114735939-9ec4bb80-9d80-11eb-8d47-5cc194aa2eab.png)

마지막으로, live view에 보내고 싶은 playground page 안의 객체가 있을 때,  
그냥 sendValue함수를 호출해서 그 객체를 함수의 매개변수로 보내면 된다.  
(pagescontents.swift의 hidden-code block에서)



요렇게 하면 auxliliary source file에 적은 코드가   
값을 다른 live view로 보내기 위해 playground page에 적어야 하는 코드를 간단하게 만들어 준 모습을 볼 수 있다!

더 자세한 내용은 [여기](https://developer.apple.com/documentation/swift_playgrounds)를 참고하라고 한다...

