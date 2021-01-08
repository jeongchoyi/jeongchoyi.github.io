---
layout: post
title: "TableHeaderView 실습" 
date: 2021-01-08
category: read 
excerpt: ""

---

# TableHeaderView 실습

![image](https://user-images.githubusercontent.com/28949235/103861685-25725880-5101-11eb-98a5-d13d5753d51a.png)

[여기](https://iamcho2.github.io/2021/01/08/scrollview-and-tableview) 에서 이어지는 내용...

내가 section header를 안 썼으면 Home_Night 뷰를 section header에 넣는 방법도 생각 해 봤겠지만,  
이미 깊이를 표시하느라 section header를 사용하고 있었다.  
그래서 검색 중 알게 된 table header view!  
section header랑은 별개로, table에 header를 붙일 수 있다. (물론 footer도 가능)

![image](https://user-images.githubusercontent.com/28949235/103975500-9e33ec00-51b7-11eb-9319-acc11956dd93.png)

요런 느낌으로 ~  
근데 내가 만들 table header view는 한 화면을 차지해야 하는 큰(?) 뷰라서,  
스토리보드에서 같이 작업하기 보다는 xib view를 만들어서 붙여주는 게 나을 것 같다고 생각했다.  

## xib view 만들기

![image](https://user-images.githubusercontent.com/28949235/103975743-43e75b00-51b8-11eb-84a6-1c92542153fe.png)

우선... 붙여보기부터 할거니까 그냥 uiview 하나 던져두기...  
클래스 이름 등 다 지정 해주고~! 

## ViewController.swift

뷰컨 파일에서 해줘야 할 것들

### xib register

```swift
let DayView = Bundle.main.loadNibNamed("DayHomeView", owner: self, options: nil)?.last as! UIView
```

### tableHeaderView 지정

``` swift
tableView.tableHeaderView = DayView 
```

물론 tableView의 IBOutlet 연결 된 이름으로,,



끝이다,,ㅋ ㅋ

### tableHeaderView 높이 지정

```swift
tableView.tableHeaderView?.frame.size.height = 어쩌구 저쩌구
```

해 주면 된다 ~.~

이렇게 쉬운 거였다니~!