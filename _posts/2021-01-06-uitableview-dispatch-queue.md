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



해석이 오빠가 알려준 거  
UI변경사항은 main thread에서 수행해야 안전함  
값은 바뀌어있는데 반영을 해주는게 보장이 안 되기 때문이라고 한다,,  
ㅠ_ㅠ 어려워

