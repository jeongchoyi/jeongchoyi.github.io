---
layout: post
title: "UILabel text 부분 bold 처리" 
date: 2021-01-12
category: read 
excerpt: ""

---

# UILabel text 부분 bold 처리

```swift
let boldText = "Filter:"
let attrs = [NSAttributedString.Key.font : UIFont.boldSystemFont(ofSize: 15)]
let attributedString = NSMutableAttributedString(string:boldText, attributes:attrs)

let normalText = "Hi am normal"
let normalString = NSMutableAttributedString(string:normalText)

attributedString.append(normalString)
```

````swift
label.attributedText = attributedString
````

