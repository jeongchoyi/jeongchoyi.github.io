---
layout: post
title: "ScrollView + TableView 실습2" 
date: 2021-01-08
category: read 
excerpt: ""

---

# ScrollView + TableView 실습2

![image](https://user-images.githubusercontent.com/28949235/103861685-25725880-5101-11eb-98a5-d13d5753d51a.png)

[여기](https://iamcho2.github.io/2021/01/07/scrollview-and-tableview) 에서 이어지는 내용...

결론부터 말하자면 저렇게 구현하면 **안 된다.**  Apple 왈(2013년이긴 함):

```
Important: You should not embed UIWebView or UITableView objects in UIScrollView objects. 
If you do so, unexpected behavior can result because touch events for the two objects can be mixed up and wrongly handled.
```

ㅇㅇ.. 그러더라 ㅋ ㅋ..  
아예 안 동작하는 건 아니였지만, 스크롤이 이중으로 생겨서 ux적으로든 뭐든 좀 불편했다.  

> 근데 swift5부터는 되는 것 같기도 하더라.
>
> ```
> Update for 2020, swift 5.X that allows your tableview inside a scrollview!
> ```
>
> 1. Create a subclass of UITableView:
>
>    ```objectivec
>     import UIKit
>    
>     final class ContentSizedTableView: UITableView {
>         override var contentSize:CGSize {
>             didSet {
>                 invalidateIntrinsicContentSize()
>             }
>         }
>    
>         override var intrinsicContentSize:CGSize {
>             layoutIfNeeded()
>             return CGSize(width: UIView.noIntrinsicMetric, height: contentSize.height)
>         }
>     }
>    ```
>
> 2. Add a UITableView to your layout and set constraints on all sides. Set the class of it to ContentSizedTableView.
>
> 3. You should see some errors, because Storyboard doesn't take our subclass' `intrinsicContentSize` into account. At runtime it will use the override in our ContentSizedTableView class
>
> 라는 [StackOverflow의 답변](https://stackoverflow.com/questions/17121488/how-to-use-uitableview-inside-uiscrollview-and-receive-a-cell-click)이 있었는디... 보편화된 방법은 아직 아닌 것 같다.

UITableView가 어차피 UIScrollView를 상속받은 놈이라 쓸 필요가 없긴 한데...  
그럼 내 뷰는 어떻게 구현하냐고 ? -.- ??

정답은,,, **TableHeaderView** 였다 ~!! 흠 근디 따로 작성해야 할 듯

