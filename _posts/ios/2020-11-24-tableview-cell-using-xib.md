---
layout: post
title: "UITableView cell로 xib 연결하기" 
date: 2020-11-24
category: read 
excerpt: ""

---

# UITableView cell로 xib 연결하기

```swift
// Register the xib for tableview cell
let cellNib = UINib(nibName: "NewsTableViewCell", bundle: nil)
self.mainTableView.register(cellNib, forCellReuseIdentifier: "newsTableViewCell")
```

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
	if let cell = tableView.dequeueReusableCell(withIdentifier: "newsTableViewCell") as? NewsTableViewCell {

	// Pass the data to colletionview inside the tableviewcell
  let rowArray = newsArray.objectsArray
            cell.updateCellWith(row: rowArray)

  return cell
	}
return UITableViewCell()
}
```

