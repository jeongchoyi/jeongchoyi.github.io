---
layout: post
title: "UISearchBar랑 UITableView 연동하기" 
date: 2021-05-06
category: read 
excerpt: ""

---

# UISearchBar랑 UITableView 연동하기

![image](https://user-images.githubusercontent.com/28949235/117280011-2348bc80-ae9d-11eb-9575-1ca54da29985.png)

오브젝트 라이브러리에 있었다...  
찾아보니까 search bar를 table view안에 넣어야 한다 뭐 이런 말이 있던데...  
우선 나는 search bar를 programmatically 하게 생성한 상태이기 때문에 ㅠ 일단 고 ㅠ

[Docs](https://developer.apple.com/documentation/uikit/uisearchbar)를 먼저 읽어보자.

>  A search bar doesn’t actually perform any searches. You use a delegate, an object conforming to the [`UISearchBarDelegate`](https://developer.apple.com/documentation/uikit/uisearchbardelegate) protocol, to implement the actions when the user enters text or clicks buttons.

ㅇㅋ... `self.searchBar.delegate = self` 해줘야겠군 delegate 함수들을 살펴보자!



### 검색하기 버튼 click - searchBarSearchButtonClicked

`searchBarSearchButtonClicked` 함수다.

```swift
extension SearchViewController: UISearchBarDelegate {
    
    func searchBarSearchButtonClicked(_ searchBar: UISearchBar) {
        print("search")
    }
}
```

요렇게만 하면

 ![image](https://user-images.githubusercontent.com/28949235/117281929-217ff880-ae9f-11eb-8259-cbce434ebe9f.png)

키보드의 검색 버튼 눌렀을 때만 search log가 찍혀서,  
내가 추가해준 검색바의 아이콘도 검색 버튼 역할을 시켜주기 위해 

```swift
// 검색 버튼
let search = UIBarButtonItem(systemItem: .search, primaryAction: UIAction(handler: { _ in
	// search action
	self.searchBarSearchButtonClicked(searchBar)
}))
```

요 부분을 추가해 줬다. (delegate 함수를 이렇게 막 갖다가 호출해도 되는지는 모르겠음)  
동작은 잘 되긴 한다 .,, ㅋ ㅋ

