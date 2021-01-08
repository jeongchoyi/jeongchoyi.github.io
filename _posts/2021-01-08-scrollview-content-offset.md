---
layout: post
title: "ScrollView Content Offset 쉽게 다루기" 
date: 2021-01-08
category: read 
excerpt: ""

---

# ScrollView Content Offset 쉽게 다루기

![image](https://user-images.githubusercontent.com/28949235/103987373-bb74b480-51cf-11eb-8103-e958ef86113f.png)

이렇게 위에 status bar, navigation bar가 있는 앱에서 scrollview를 쓸 때, scrollview의 contentOffset을 쓸 일이 생긴다.

![image](https://user-images.githubusercontent.com/28949235/103987404-c596b300-51cf-11eb-8151-4ec086df7347.png)

사용을 위해 y축의 contentOffset값을 찍어보면,

![image](https://user-images.githubusercontent.com/28949235/103987413-c9c2d080-51cf-11eb-8438-a6c7d6b470cc.png)

화면이 그냥 default 상태, 즉 앱을 처음 켰을 때 상태인데도 -94가 찍히는 모습을 볼 수 있다.  
status bar, navi bar, 다른 inset값들이 포함된 값이라 그런 것 같은데...  
나는 그냥 앱 딱 처음 실행됐을 때 보이는 뷰의 y축 offset값이 0이였으면 좋겠는거지!

왜냐면.. 그게 작업하기 오조오억배 편하니까...

```swift
tableView.contentInsetAdjustmentBehavior = .never
```

요 놈을 넣어주면 해결된다.



### contentInsetAdjustmentBehavior 설정 전

![image](https://user-images.githubusercontent.com/28949235/103987705-4190fb00-51d0-11eb-8cc2-b8a54033e6ce.png)

저렇게 status bar, navi bar 밑에서부터 view가 시작된다.  
그러니까 처음 뷰가 열렸을 때 offset값은 음수 값이 나오는 것!

### contentInsetAdjustmentBehavior 설정 후

![image](https://user-images.githubusercontent.com/28949235/103987835-6f763f80-51d0-11eb-9b66-5881f597501a.png)

처음부터 뷰가 시작되기 때문에  
처음 뷰가 화면에 보일 때 offset값이 0이다.  
직관적으로 작업할 수 있게 돼서 너무너무 편한 거쥐,,~

> 추가로, 나는 저 사진에서 선택된 view의 높이를 기기별 화면 크기에 맞춰야 했기 때문에  
> UIScreen.main.bounds.height 를 사용했는데, 이게 contentInsetAdjustmentBehavior 설정 전에는  
> UIScreen.main.bounds.height에서 navi bar height, status bar height 등을 빼 줘야  
> 내가 원하는 대로 동작했었다.  
>
> contentInsetAdjustmentBehavior 설정 후에는 그냥 UIScreen.main.bounds.height만 해 줘도  
> 딱 맞음 ~~ ^___________________________^

![image](https://user-images.githubusercontent.com/28949235/103988073-d267d680-51d0-11eb-9987-56f079d824b3.png)