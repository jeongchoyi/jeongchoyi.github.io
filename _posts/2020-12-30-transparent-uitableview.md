---
layout: post
title: "UITableView 투명하게" 
date: 2020-12-30
category: read 
excerpt: ""

---

# UITableView 투명하게

### 1. viewDidLoad()

```swift
self.tableview.backgroundColor = UIColor.clear
```

### 2. UITableViewDelegate - willDisplay()

```swift
func tableView(_ tableView: UITableView, willDisplay cell: UITableViewCell, forRowAt indexPath: IndexPath) {
        cell.backgroundColor = UIColor.clear
}
```

### 3. 셀 선택 시에도 투명하게

![image](https://user-images.githubusercontent.com/28949235/103336206-ae164680-4aba-11eb-8cab-bb85ddd3e1c2.png)

