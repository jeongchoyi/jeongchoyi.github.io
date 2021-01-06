---
layout: post
title: "UITableView DispatchQueue" 
date: 2021-01-06
category: read 
excerpt: ""

---



```swift
DispatchQueue.main.async {
            for index in 0 ..< tableView.visibleCells.count {
                let zPosition = CGFloat(tableView.visibleCells.count - index)
                tableView.visibleCells[index].layer.zPosition = zPosition
            }
        }
```

얘가 뭔 짓을 해줬길래.. 오류가 해결됐나...!!! 를 이제 고민해보자...

우선 이 함수는 didEndDisplaying 함수 내부에 있다.

```swift
    func tableView(_ tableView: UITableView, didEndDisplaying cell: UITableViewCell, forRowAt indexPath: IndexPath) {
        DispatchQueue.main.async {
            for index in 0 ..< tableView.visibleCells.count {
                let zPosition = CGFloat(tableView.visibleCells.count - index)
                tableView.visibleCells[index].layer.zPosition = zPosition
            }
        }
    }
```

먼저 cell life cycle부터 살펴보고 오자... [여기](https://jinnify.tistory.com/58)를 참고했다. 나도 따로 정리 해 놔야 할듯..



