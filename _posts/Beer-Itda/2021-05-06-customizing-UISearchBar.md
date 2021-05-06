---
layout: post
title: "UISearchBar 커스텀하기" 
date: 2021-05-06
category: read 
excerpt: ""

---

# UISearchBar 커스텀하기

![image](https://user-images.githubusercontent.com/28949235/117267746-90565500-ae91-11eb-9fa8-eb2807a32430.png)

검색하기 뷰를 만들어 봅시다!

![image](https://user-images.githubusercontent.com/28949235/117268081-e0351c00-ae91-11eb-8e8d-50e7d1a01b59.png)

우선 VC에 Navigation Controller embed 해주고 시작

### init - 1(안씀)

```swift
let searchController = UISearchController(searchResultsController: nil)
self.navigationItem.searchController = searchController
```

요 두줄을 viewDidLoad()에 넣어준다.

![image](https://user-images.githubusercontent.com/28949235/117268484-4621a380-ae92-11eb-811d-ca80ef329d19.png)

그럼 이렇게 Search bar가 생긴다!

![image](https://user-images.githubusercontent.com/28949235/117268526-50dc3880-ae92-11eb-9513-b90075cf46f3.png)

클릭하면 애니메이션과 함께 Cancel 버튼도 생긴다.

### 스크롤 도중에도 search bar 보이게 하기

```swift
self.navigationItem.hidesSearchBarWhenScrolling = false
```

>  근데 난 애니메이션도 필요 없고 오른쪽 Cancel 버튼도 다 필요 없다..!  
> 그래서 그냥 UISearchBar를 만들어서 붙이는 식으로 할까 고민했는데,  
> 뭐가 더 나은 방법인지는 모르겠다.  
> UISearchDisplayController는 UISearchController가 나오기 전 쓰던 옛날 방법이라 안 쓰는 게 맞는 것 같은데...  ~!
>
> cancel 버튼은 UISearchController 프로퍼티 써서 없앨 수 있는데,  
> 애니메이션을 없애려면 UISearchController 말고 UISearchBar를 써야하는 것 같아서 방법 변경...

### init - 2

```swift
let searchBar = UISearchBar() 
searchBar.placeholder = "맥주를 검색해보세요" 
self.navigationItem.titleView = searchBar
```

![image](https://user-images.githubusercontent.com/28949235/117273672-4ff9d580-ae97-11eb-86a1-a5da041fb739.png)

원하는 대로 나오긴 하는데, delegate 등에 있어서 UISearchController보다  
불편한 게 있을 것 같아서 걱정된다..,, 우선 고고

### 버튼 만들기

![image](https://user-images.githubusercontent.com/28949235/117273817-6e5fd100-ae97-11eb-8102-63827f07bb42.png)![image](https://user-images.githubusercontent.com/28949235/117273893-7fa8dd80-ae97-11eb-9d6a-d130a9f27f4a.png)

오른쪽에 magnifying glass 버튼이 있어야 하고, 텍스트 editing 상태일 때 clear button도 있어야 한다.

![image](https://user-images.githubusercontent.com/28949235/117274117-b7b02080-ae97-11eb-8297-222e28fb578f.png)

clear button은 editing 상태일 때 자동으로 생긴다.

```swift
private func initSearchBar() {
    // 검색 버튼
    let search = UIBarButtonItem(systemItem: .search, primaryAction: UIAction(handler: { _ in
        // search action
    }))
    self.navigationItem.rightBarButtonItem = search
    let searchBar = UISearchBar(frame: CGRect(x: 0, y: 0, width: view.frame.width - 80, height: 0))
    searchBar.placeholder = "맥주를 검색해보세요"
    self.navigationItem.leftBarButtonItem = UIBarButtonItem(customView: searchBar)
}
```

`width: view.frame.width - 80` 이게 문제인데...  
우선 80을 빼 주니까

![image](https://user-images.githubusercontent.com/28949235/117275659-38235100-ae99-11eb-8a93-9d9f791712f9.png)

여러 디바이스에서 잘 나오긴 한다. 70 뺐을 땐 아이콘 크기가 좁쌀만했고,  
그거보다 더 작게 뺐을 땐 그냥 안나왔다. systemItem의 크기가 몇인질 모르겠어서 ㄱ-,,, 찝찝..

### 디자인 커스텀하기

1. 왼쪽 돋보기 아이콘 없애기

   ```swift
   searchBar.setImage(UIImage(), for: UISearchBar.Icon.search, state: .normal)
   ```

2. clear button 아이콘 변경 (우선 난 안함, 추후에 필요하면 사용할 것)

   ```swift
   searchBar.setImage(UIImage(named: "icCancel"), for: .clear, state: .normal)
   ```

3. 오른쪽 돋보기 아이콘 색상 바꾸기

   ```swift
   self.navigationItem.rightBarButtonItem?.tintColor = UIColor.black
   ```

4. textfield 배경색 없애기

   ```swift
   searchBar.searchTextField.backgroundColor = UIColor.clear
   ```

5. textfield 글자 크기

   ```swift
   searchBar.searchTextField.font = UIFont.systemFont(ofSize: 14)
   ```

![image](https://user-images.githubusercontent.com/28949235/117278174-7c175580-ae9b-11eb-8afa-b996451e94ae.png)



### 출처

[fomaios](https://fomaios.tistory.com/entry/%EC%84%9C%EC%B9%98%EB%B0%94-%EC%BB%A4%EC%8A%A4%ED%85%80%ED%95%98%EA%B8%B0-Custom-UISearchBar) [zeddios](https://zeddios.tistory.com/1196?category=682195)

