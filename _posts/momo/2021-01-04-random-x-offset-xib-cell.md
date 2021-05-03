---
layout: post
title: "xib cell NSLayoutConstraint" 
date: 2021-01-04
category: read 
excerpt: ""

---

# xib cell NSLayoutConstraint 값 전달하기

Bubble의 위치값을 랜덤(0~9)으로 받아오는데,  
xib로 구현해놓은 cell으로 그 값을 전달해서 각 cell마다 leading값을 다르게 지정해주는 과정,,

0~9로 받아오고 실제 표현은 뷰의 width에 맞게 보정해서 사용해야 함!

### IBOutlet

![image](https://user-images.githubusercontent.com/28949235/103523296-fe2c4900-4ebe-11eb-8010-4afd545ecbab.png)

이 Constraints를 xib swift 파일에 IBOutlet 연결 해 준다.

```swift
    @IBOutlet weak var bubbleLeadingOffset: NSLayoutConstraint!
```

요렇게 해주면 뷰컨에서 data 넘겨줄 때 같이 값 넘겨주고, 받으면 된다.

```swift
    func setCell(bubble: Bubble){
        dateLabel.text = bubble.date
        cateLabel.text = bubble.cate
        bubbleLeadingOffset.constant = //여기에 받아와서 값 보정하면 됨!
    }
```

아니면 값 보정하고 할당해줘도 되겠쪄 머

### 값 보정하기

```swift
func setCell(bubble: Bubble){
		dateLabel.text = bubble.date
    cateLabel.text = bubble.cate
    bubbleLeadingOffset.constant = calculateLeadingOffset(num: bubble.leadingNum)
}
    
func calculateLeadingOffset(num: Int) -> CGFloat{
    let viewWidth = UIScreen.main.bounds.width
    let leadingOffset = CGFloat(Int(viewWidth) / 10 * num)
        
    return leadingOffset
}
```

함수로 만들어서 보정해줬음

