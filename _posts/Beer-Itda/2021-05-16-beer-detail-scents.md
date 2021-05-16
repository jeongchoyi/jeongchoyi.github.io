---
layout: post
title: "데이터 개수에 따라 stack view 내 view 개수 변경하기" 
date: 2021-05-16
category: read 
excerpt: ""

---

# 데이터 개수에 따라 stack view 내 view 개수 변경하기

>  맥주 상세 향 개수 분기처리

### 만들 뷰

<img src="https://user-images.githubusercontent.com/28949235/118383737-ac998500-b63b-11eb-85fd-365281971b2c.png" alt="image" style="width:500px" />

1~3개일 때

<img src="https://user-images.githubusercontent.com/28949235/118383740-b0c5a280-b63b-11eb-886e-18bea8ef0fb0.png" alt="image" style="width:500px;" />

4개일 때



<img src="https://user-images.githubusercontent.com/28949235/118383727-8ffd4d00-b63b-11eb-919e-068f694e72a9.png" alt="image" style="width:400px" />

<img src="https://user-images.githubusercontent.com/28949235/118383723-870c7b80-b63b-11eb-91a2-92e68041e34e.png" alt="image" style="width:400px" />

향 view가 1~3개일 땐 horizontal하게 배치되고, 받아오는 데이터에서 향이 4개일 때에는 하나만 밑으로 내려가기!

### 만들기

![image](https://user-images.githubusercontent.com/28949235/118383765-ea96a900-b63b-11eb-9df4-83e090bba9e5.png)

우선 4개(최대)를 다 만들어준다.  
위쪽의 3개는 stack view로 감싸주고, 4번째 view는 그냥 uiview!

<img src="https://user-images.githubusercontent.com/28949235/118384502-73641380-b641-11eb-8a06-f598db1b8135.png" alt="image" style="width:500px;" />

첫번째 scent view외에는 다 hidden처리 해 줬다. (향 최소 1개 존재)  
나중에 데이터가 있으면 hidden만 풀어주면 되게끔!

그리고 아직 서버통신 전이니까 향 string 배열도 만들어준다. (나중에 서버에서 받아올 값)  
![image](https://user-images.githubusercontent.com/28949235/118383782-1ade4780-b63c-11eb-81f4-412d8e952265.png)

4개의 뷰, 레이블을 각각 뷰 배열, 레이블 배열에 넣어준다.

UIView, UILabel 배열을 만들 땐  
일반 자료형 배열 만들듯이 초기화 하는 게 아니라, append를 사용해서 element를 추가해야 한다.

```swift
var scentViews = [UIView]()
var scentLabels = [UILabel]()
```

 이렇게 배열을 만들고, ( `() ` 빼먹지 말기! )

```swift
scentViews.append(scentView1)
scentViews.append(scentView2)
scentViews.append(scentView3)
scentViews.append(scentView4)
        
scentLabels.append(scentLabel1)
scentLabels.append(scentLabel2)
scentLabels.append(scentLabel3)
scentLabels.append(scentLabel4)
```

요렇게 append 해주면 된다. 

```swift
for idx in 0..<scents.count {
		scentViews[idx].isHidden = false
		scentLabels[idx].text = scents[idx]
            
		if idx == 4 {
				scentView4HeightConstraint.constant = scentView4Height
		}
}
        
if scents.count != 4 {
		scentView4.isHidden = true
		scentView4HeightConstraint.constant = 0
}
```

그리고 scents 배열의 개수에 따라서 isHidden false처리 해주고, label에 텍스트도 입력해준다. 

> stackview.viewWithTag( ) 쓰려다가 그냥 배열 만들어서 처리했다 !



### 끝! 

<img src="https://user-images.githubusercontent.com/28949235/118384549-c50c9e00-b641-11eb-9e25-430cef64d444.png" alt="image" style="width:400px;" />

향 2개일 때

<img src="https://user-images.githubusercontent.com/28949235/118384559-ebcad480-b641-11eb-8d2d-5e8a837e4569.png" alt="image" style="width:400px;" />

향 4개일 때

<img src="https://user-images.githubusercontent.com/28949235/118384568-ff763b00-b641-11eb-9fc7-1efb7d6932ef.png" alt="image" style="width:400px" />

향 1개일 때

