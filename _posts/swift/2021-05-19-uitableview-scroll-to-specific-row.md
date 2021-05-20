---
layout: post
title: "UITableview 특정 row로 스크롤하기" 
date: 2021-05-19
category: read 
excerpt: ""

---

# UITableview 특정 row로 스크롤하기

```swift
let rowIndexPath = NSIndexPath(row: 이동할 row int값, section: 0)
mainTableView.scrollToRow(at: rowIndexPath as IndexPath, at: UITableView.ScrollPosition.top, animated: true)
```

간단..~

### 스크롤을 cell의 어떤 기준으로 할 것인가?

`UITableView.ScrollPosition.top` 요 부분을 수정해주면 되는데,  
top으로 지정하면 해당 셀의 맨 위 top에 맞춰서 스크롤한다.  
`.top`, `.middle`, `.bottom` 이 있음 !

<img src="https://user-images.githubusercontent.com/28949235/118981384-3e472080-b9b5-11eb-90dd-9cf7f0be34ef.gif" alt="Screen-Recording-2021-05-20-at-9 47 16-PM" width=300 />

스르륵~