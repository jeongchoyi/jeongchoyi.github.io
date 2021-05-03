---
layout: post
title: "Create and remove subviews in circular shape" 
date: 2020-11-14
category: read 
excerpt: ""

---

# subview를 원형으로 배치하고 삭제하기

솝트 27기 디자인-클라이언트 합동세미나를 진행하면서 구현한 뷰를 기록해봅니다,,,

![image](https://user-images.githubusercontent.com/28949235/99184718-5761e100-2788-11eb-89fe-6004a83251b2.png)

> 내가 맡은 뷰

### 뷰 세부사항

\+ 버튼을 누르면 원이 추가됨

원 배치는 원이 n개 있을 때 360도를 1/n 로 나눈 각도마다 한개씩

원은 최대 4개

\+ 버튼 포함 원이 3개였던 상태에서 +를 한번 더 눌러서 원이 4개가 되면 + 버튼 사라지면서 원 4개 됨



### programmatically 하게 동그란 버튼 만들기

동그란 버튼들이 사용자의 클릭에 따라 생성되어야해서,  
스토리보드를 사용하지 않고 코드상으로 버튼을 만들고 오토레이아웃을 잡아줘야 했다.

```swift
// + 버튼 생성 함수
func createPlusButton(size: CGFloat) -> UIButton {
  	
  	//버튼 배경 이미지 변수 생성
    let image = UIImage(named: "3RdMindmapButtonIos") as UIImage?
  
    let button = UIButton(type: .custom)
    //버튼 배경 이미지 지정: + 버튼은 위에 text가 없어서 바로 이미지로 지정
    button.frame = CGRect(x: 100, y: 100, width: 93, height: 93)
    button.setImage(image, for: .normal)
  
    //Action 함수 지정: 아래 @objc 함수
    button.addTarget(self, action: #selector(self.plusBtnTouched), for:.touchUpInside)
  
    //크기 지정
    button.translatesAutoresizingMaskIntoConstraints = false
    button.widthAnchor.constraint(equalToConstant: size).isActive = true
    button.heightAnchor.constraint(equalToConstant: size).isActive = true
  
    //원형 모양 만들기
    button.layer.cornerRadius = size / 2
  
    //삭제를 위한 tag 지정
    button.tag = 1

    return button
}
```

이런식으로 mindmap 버튼, 중간에 위치한 큰 버튼 (아직 서버 통신 전이라 우선 버튼으로 만듦)을 만드는  
함수를 작성해주면 된다.



### 버튼 원형으로 배치하기

제일 헉 했던 부분... 원형으로 view를 배치해본 적이 없었는데 이번 기회에 해보게 됨 !  
스택오버플로우 만세 ~!

```swift
func setUpButtons(count: Int, around center: UIView, radius: CGFloat) {
    //각 버튼간의 각도 차이 계산
    let degrees = 360 / CGFloat(count)
산
    for i in 0 ..< count {
       // +플러스 버튼 1개 생성
       if(i == 0){
           let plusButton = createPlusButton(size: 93)
           self.view.addSubview(plusButton)
           
           //삼각법을 사용해서 중심에서 가로, 세로 얼마나 떨어질건지 계산
           let hOffset = radius * cos(CGFloat(i) * degrees * .pi / 180)
           let vOffset = radius * sin(CGFloat(i) * degrees * .pi / 180)
           
           //centerY anchors, offsets 사용해서 x,y 위치 지정
           plusButton.centerXAnchor.constraint(equalTo: center.centerXAnchor, constant: hOffset).isActive = true
           plusButton.centerYAnchor.constraint(equalTo: center.centerYAnchor, constant: vOffset).isActive = true
         
        // 나머지 mindmap 버튼 생성   
        }else{
           let mindButton = createMindButton(size: 116)
           self.view.addSubview(mindButton)

           let hOffset = radius * cos(CGFloat(i) * degrees * .pi / 180)
           let vOffset = radius * sin(CGFloat(i) * degrees * .pi / 180)

           mindButton.centerXAnchor.constraint(equalTo: center.centerXAnchor, constant: hOffset).isActive = true
           mindButton.centerYAnchor.constraint(equalTo: center.centerYAnchor, constant: vOffset).isActive = true
        }
    }
}
```

> 4개가 될때는 if문 없는 `setUpButtonsWithoutPlusButton` 함수를 만들어서 호출되게 함



### 버튼 추가하기

```swift
// #selector로 함수를 지정했기 때문에 @objc 키워드 달아줘야 함
@objc func plusBtnTouched(){
  	//현재 버튼 개수
    currentBtnNum += 1
    
    //view에다 tag를 걸어서 특정 tag 뷰만 remove하는 반복문 작성
    for view in self.view.subviews {
        if(view.tag == 1 || view.tag == 2){
            view.removeFromSuperview()
        }
    }
    
    //버튼 수에 따라 다시 버튼들 생성
    if(currentBtnNum < 4){
        setUpButtons(count: currentBtnNum, around: self.circleImageView, radius: 150)
    }else{
        setUpButtonsWithoutPlusButton(count: currentBtnNum, around: self.circleImageView, radius: 150)
    }
}
```

> +플러스 버튼과 mindmap 버튼에 tag 1,2번을 걸어놨다  
> 이렇게 각 뷰에 tag를 걸어주면 원하는 tag가 달린 뷰들을 일괄로 삭제하는게 가능하다

<img src="https://user-images.githubusercontent.com/28949235/99185525-bece5f80-278d-11eb-860b-104207ef6020.gif" alt="KakaoTalk_Video_2020-11-15-21-57-21" width="300" />

완성 ~