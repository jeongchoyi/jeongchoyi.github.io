---
layout: post
title: "UITextView 펼치기 접기" 
date: 2021-05-16
category: read 
excerpt: ""

---

# UITextView 펼치기 접기

![image](https://user-images.githubusercontent.com/28949235/118385208-fee0a300-b647-11eb-9392-6273055656af.png)

* 최대 3줄
* 3줄 넘어갈 시 더보기 버튼 표시 O
* 3줄 이하일 때 더보기 버튼 표시 X
* 더보기, 접기 가능 (펼치기 & 접기)

### 스토리보드

![image](https://user-images.githubusercontent.com/28949235/118385318-d73e0a80-b648-11eb-9c58-8a29f8ba9f36.png)

에는 글을 이렇게 길게 써놓고... (서버 통신 하면 서버에서 받아오는 내용)  

### init

```swift
var reviewTextViews = [UITextView]()
private func initReviewTextViews() {   
		reviewTextViews.append(reviewTextView1)
		reviewTextViews.append(reviewTextView2)
		reviewTextViews.append(reviewTextView3)
        
		for idx in 0..<3 {
				reviewTextViews[idx].textContainer.maximumNumberOfLines = 3
				reviewTextViews[idx].textContainer.lineBreakMode = .byTruncatingTail
		}
}
```

`maximumNumberOfLines` 를 3으로 지정만 해줘도 알아서 잘라준다.

![image](https://user-images.githubusercontent.com/28949235/118385347-02285e80-b649-11eb-8850-010297350e81.png)

귯~

어차피 더보기 버튼때문에 가려서 안 보이겠지만 truncate 처리도 해줬다.

### 3줄 이하일 땐 더보기 버튼 숨기기

현재 텍스트뷰의 높이를 구하는 방법 !은  
따로 프로퍼티는 없고 (아마) 전체 textContainer의 높이를 font의 lineheight로 나눠주면 된다. ^^  
말그대로 전체 높이를 폰트 몇 줄이 차지하는가 ㅋㅋㅋ

```swift
let lineCount = (reviewTextViews[idx].contentSize.height - reviewTextViews[idx].textContainerInset.top - reviewTextViews[idx].textContainerInset.bottom) / reviewTextViews[idx].font!.lineHeight
```

이 때 textContainerInset도 고려해줘야 깔끔하게 값이 나온다.

```swift
private func initMoreButtons() {
        
		for idx in 0..<3 {
            
				let lineCount = (reviewTextViews[idx].contentSize.height - 		reviewTextViews[idx].textContainerInset.top - reviewTextViews[idx].textContainerInset.bottom) / reviewTextViews[idx].font!.lineHeight
            
				if lineCount <= 3 {
						reviewMoreButtons[idx].isHidden = true
				}
    }
}
```

근데 이 함수를 `viewDidLoad()`에 넣으면 안된다!  
왜냐하면, textView의 autolayout constraint들이 아직 제대로 세팅되지 않은 상태이기 때문 ~~  
그래서 `viewDidAppear()`에 넣어줘야 한다. 

> viewWillAppear 🔜 viewWillLayoutSubviews 🔜 viewDidLayoutSubviews이 constraint를 만족시키기 위해 위해 차례대로 호출되는데, 따라서 그 다음으로 불리는 viewDidAppear에서야 constraint가 잘 세팅되어 있을 것  
> -갓수진님의 말씀-

### 펼치기 접기

@IBAction 연결을 다 해준다.

```swift
    // MARK: - @IBAction Functions

    @IBAction func touchMoreButton1(_ sender: UIButton) {
        sender.isSelected = !sender.isSelected
        
        if sender.isSelected {
            // 펼치기
            reviewTextView1.textContainer.maximumNumberOfLines = 0
            reviewTextView1.invalidateIntrinsicContentSize()
        } else {
            // 접기
            reviewTextView1.textContainer.maximumNumberOfLines = 3
            reviewTextView1.invalidateIntrinsicContentSize()
        }
    }
```

2, 3도 마찬가지 ~~  
`maximumNumberOfLines`의 limit을 없애고 싶으면 0을 주면 된다.  
그리고, `maximumNumberOfLines`만 조절하면 UI에는 반영이 되지 않는데,  
`invalidateIntrinsicContentSize()`를 호출해줘야 한다.

`intrinsicContentSize`에 변경이 생겼을 때 UIKit에게 알려주는 역할!



추가로, isSelected를 쓰는거다보니 버튼이 selected 상태일 때 UI가 거지같이 바뀌는데 (^^)  
스토리보드에서 state config를 selected로 바꾸고 selected 상태일 때의 UI를 지정해주면 된다.  
난 버튼 타이틀, 배경색, 텍스트색을 다시 지정해줬다! (기본은 색이 반전되는 것)

### 끝!

![Screen-Recording-2021-05-16-at-2 08 13-PM](https://user-images.githubusercontent.com/28949235/118386152-43703c80-b650-11eb-922b-25492c02f4af.gif)