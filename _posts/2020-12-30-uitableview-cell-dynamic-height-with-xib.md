---
layout: post
title: "xib 사용하는 UITableView Cell 유동적으로 높이 설정하기" 
date: 2020-12-30
category: read 
excerpt: ""

---

# xib UITableView Cell 유동적으로 높이 설정하기

### 1. xib cell autolayout

![image](https://user-images.githubusercontent.com/28949235/103328187-1c4b1100-4a9b-11eb-9208-f70b1c97fc56.png)

height 값 autolayout 안 잡고 상하좌우 (필요에 따라 알아서 겠지만.. bottom은 필수고) 잡아줌

### 2. ViewController.swift

```swift
// outside
@IBOutlet weak var tableView: UITableView!
let quoteCellReuseIdentifier = "quoteCellReuseIdentifier"
    
let quotes = [
        "I always was a crybaby, wasn’t I",
        "Do not worry about me. Someone has to take care of these flowers.",
        "They were the one that wanted to... ...to use our full power. I was the one that resisted. And then, because of me, we... Well, that's why I ended up a flower",
        "But I can feel every other monster's as well. They all care about each other so much. And... they care about you too, Frisk. I wish I could tell you how everyone feels about you.",
        
        "As a flower, I was soulless. I lacked the power to love other people. However, with everyone's souls inside me... I not only have my own compassion back... But I can feel every other monster's as well."
]
```

### viewDidLoad()

```swift
// xib register
self.tableView.register(UINib(nibName: String(describing: QuoteTableViewCell.self), bundle: nil), forCellReuseIdentifier: quoteCellReuseIdentifier)
// 요기 부분이 핵심 !!!
self.tableView.rowHeight  = UITableView.automaticDimension
self.tableView.estimatedRowHeight = 80
// delegate
self.tableView.dataSource = self
self.tableView.delegate = self
```

### DataSource, Delegate

![image](https://user-images.githubusercontent.com/28949235/103328260-76e46d00-4a9b-11eb-9112-cf57e719e5a1.png)

`HeightForRowAt()` 없이~

## 결과

![image](https://user-images.githubusercontent.com/28949235/103328311-c0cd5300-4a9b-11eb-8935-48370e8e3f4e.png)