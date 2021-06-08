---
layout: post
title: "당근마켓 클론코딩 - 1. UITableView Sticky header 구현하기" 
date: 2020-11-2
category: read 
excerpt: ""

---

### 당근마켓 클론코딩 - 1

[당근마켓 클론코딩 1주차](https://github.com/iamcho2/iOS-clone-coding-study/blob/main/carrot-market/README.md) 에서 이어지는 내용

# UITableView - Sticky header 야매로 구현하기

**\+ 스크롤에 따라 alpha값 조절하기**

<img src="https://user-images.githubusercontent.com/28949235/97891039-a1eb6280-1d71-11eb-8582-de6a20727883.gif" alt="201102-1" width=300px />

당근마켓 클론코딩을 하다가 이게 하고싶어서.. 구현하는 과정을 기록하려고 한다 ✏️

그냥 Sticky header가 아니라,, 위에 첫번째 뷰는 사라져야하고 두번째 뷰는 sticky 해야 해서.. 이렇게 구현했다 ,,

나중에 언젠가 써먹을 날이 올 듯 ? ( ◠‿◠ )..

간단히 먼저 설명하자면... table cell이 나오는 부분의 윗부분을 모조리 headerView라고 하고

스크롤에 따라 headerView에 max,min height값을 주는 것!

### 결과물 먼저 보기

![AnyConv](https://user-images.githubusercontent.com/28949235/97892201-107cf000-1d73-11eb-963c-8574dbaa8282.gif)

화질이 와이라노지만.. 성공~! 

## 뷰 짜기

![image](https://user-images.githubusercontent.com/28949235/97892774-cb0cf280-1d73-11eb-985c-0aa98319f13c.png)

View Controller에 Table View 올려주고 Prototyle Cells까지 다 작업해 준 후에

![image](https://user-images.githubusercontent.com/28949235/97892880-ef68cf00-1d73-11eb-8d67-b12e3f6e999a.png)

상단에서 움직일 / 고정될 뷰를 올려준다

근데 이때! view가 TableView에 들어가도록 view를 넣는게 아니라 그냥 위에 얹는거임

![image](https://user-images.githubusercontent.com/28949235/97892630-97ca6380-1d73-11eb-8d6d-cbdc570cc469.png)

> **MainTableView** : 테이블 뷰
>
> **HeaderView** : 상단 전체 뷰
>
> ​	그리고 그 안에 **UpperHeaderView** : alpha값 조절할 view, **View** : segmented control이 있는 뷰(인데 저 부분은 아직 작업 안 해서 저 상태)

MainTableView랑 HeaderView가 같은 hierarchy에 있어야 함!

오토레이아웃은 알아서 하는데 **headerView에 height constraint는 필수**! 나중에 이걸 **NSLayoutConstraint**에 연결해서 쓸거라서..

글고.. 데이터 불러오고 Model 만들고 이런것도 다 했다 치고 ~



## outlet 연결, 변수 선언

```swift
let maxHeight: CGFloat = 100.0 //headerView의 최대 높이값
let minHeight: CGFloat = 100.0 //headerVIew의 최소 높이값

@IBOutlet weak var mainTableView: UITableView!{
    didSet {
        mainTableView.contentInset = UIEdgeInsets(top: maxHeight, left: 0, bottom: 0, right: 0)
     }
}
@IBOutlet weak var headerView: UIView!
@IBOutlet weak var upperHeaderView: UIView!
}
```

배치했던 view들 outlet 선언해 줌



아까 위에서 HeaderView에 잡아준 height constraint를 outlet연결 해 줄건데, (스크롤하는거에 따라 headerView의 높이를 바꿔줄거라서)

```swift
@IBOutlet weak var heightConstraint: NSLayoutConstraint! {
    didSet {
        heightConstraint.constant = maxHeight
    }
```

먼저 코드를 쓰고 아래처럼 드래그해서 연결해주면 된다

![AnyConv com__Screen Recording 2020-11-03 at 1 43 25 AM](https://user-images.githubusercontent.com/28949235/97894630-11fbe780-1d76-11eb-8141-6ce74e4b2def.gif)



## scrollViewDidScroll() 메소드 구현

tableView가 scroll 됨에 따라 UI가 바뀌어야되므로 scrollViewDidScroll() 메소드를 구현해줄건데,

 이 함수는 UIScrollViewDelegate의 메소드다. 스크롤이 됐을 때 마다 호출됨 - 쓰윽 스크롤해도 몇십번 호출 돼 있음

```swift
extension MainTableVC: UITableViewDelegate {
  func scrollViewDidScroll(_ scrollView: UIScrollView) {
   	//아래 나오는 내용들은 다 이 함수 안에 들어가 있는 코드
  }
}
```



### 스크롤에 따라 특정 뷰의 특정 부분까지만 숨기기

```swift
if scrollView.contentOffset.y < 0 {
    heightConstraint.constant = max(abs(scrollView.contentOffset.y), minHeight)
} else {
    heightConstraint.constant = minHeight
}
```



### 스크롤에 따라 특정 부분 alpha값 서서히 조절하기

자자.. 먼저 해야 하는 것은 offset을 계산하는 것~!

![AnyConv](https://user-images.githubusercontent.com/28949235/97892201-107cf000-1d73-11eb-963c-8574dbaa8282.gif)

```swift
let offset = -scrollView.contentOffset.y
let percentage = (offset-100)/50
upperHeaderView.alpha = percentage
```

이렇게 해주면.. 끝... ㄱ- ~

`let percentage = (offset-100)/50` 이 부분은 offset값에 따라 내가 원하는 alpha 값을 얻기 위해 계산한거라.. 

기존 뷰 높이에 따라 달라집니다..



도움받은 자료들, 검색하다 주운 자료들

https://stackoverflow.com/questions/46692316/how-to-hide-a-uiview-while-scrolling

https://stackoverflow.com/questions/31857333/how-to-get-uiscrollview-vertical-direction-in-swift

https://stackoverflow.com/questions/25263343/how-to-change-alpha-value-along-with-scrolling

https://stackoverflow.com/questions/43864557/scrolling-resizing-uitableview