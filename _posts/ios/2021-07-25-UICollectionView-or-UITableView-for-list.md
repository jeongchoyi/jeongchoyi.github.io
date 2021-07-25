---
layout: post
title: "앞으로 UITableView 쓰지 말라고?" 
date: 2021-07-25
category: read 
excerpt: ""

---

# 앞으로 UITableView 쓰지 말라고?

### 서론

이번에 쟈니를 개발하면서 

<img src="https://user-images.githubusercontent.com/28949235/126870895-e432e3db-4080-4d03-9d3b-4b1dc8f6f534.jpeg" alt="IMG_21CE4BCFF69D-1" width=300 />

이 뷰를 CollectionView를 써서 구현했는데, 처음에 뷰를 어떻게 짤지 설계를 할 때에는  
Table View를 써서 구현하려고 했었다. 근데 막상 cell 간의 여백 등을 고려하려고 하니까  
table view 보다는 collection view가 오히려 편하겠다는 생각이 들어서 결국 collection view로 구현했다.

딱 봤을 땐 단일 열의 list 구조라 table view를 생각하기 쉬운데,  
collection view로도 충분히 구현 가능하다..? 오히려 더 편할때가 있다,,?

그럼 그냥 table view 안 쓰고 collection view로 다 구현하면 안 되나 !?!? 싶어서 찾아봤다.

### SOF

너무 당연하게도 [스택오버플로우](https://stackoverflow.com/questions/23078847/when-to-use-uicollectionview-instead-of-uitableview)에 다음과 같은 질문이 있었다.

```
Q.
UICollectionView가 UITableView의 업그레이드 된 버전이라고 iOS6때 소개된 걸 봤는데, 
그럼 내가 UITableView말고 언제 UICollectionView를 써야 돼?

아직도 UITableView를 쓰는 앱이 있던데, UICollectionView가 UITableView가 할 수 있는 것들을 다 할 수 있다면 왜 사람들은 계속 UITableView를 쓰는거야? 성능이나 뭐 다른거에서 차이점이 있는건가?
```

답변들은 아래와 같았다.  
( 질문을 내 의도와 다르게 해석한 답변들은 적지 않았다. 
 `UICollectionView는 그리드, multi-column을 구현할 수 있으니까!` 같은...)

```
A1.
보통 간단한 리스트는 UITableView, 고난이도의 커스텀은 UICollectionView 써.
소프트웨어 개발에서는 그 구현을 위해 "가능한 한 가장 간단한 것"을 고르는게 최고야.

수정: iOS14에서, UICollectionView로도 list를 만들 수 있고, 추천되는 방법이야.
이 WWDC20 세션을 보렴 ~
```

[Lists in UICollectionView](https://developer.apple.com/videos/play/wwdc2020/10026/) 라는 WWDC 세션도 있었다.. ㄷ ㄷ

```
A2.
UITableview 고를거면, 너 우선 iPad도 고려할건지 말건지부터 생각해봐.
만약에 iPad 전용 레이아웃을 원하는거면, 폰에서는 단일 열이었던 게
iPad에서는 그리드가 되는게 나을 수도 있으니까!
```

iPad에서 해당 앱을 실행했을 때 어떻게 나오길 원하는지에 따라서도 갈릴 수 있겠군,,

```
A3.
내 기준은 이래.
1. UITableView로 되는거면, 써
2. UITableView로 하려면 너무 코드가 길어지거나, 그걸로 안되면 UICollectionView를 써

둘 중 고르기 전에 UITableView의 제약을 먼저 생각해 봐야 해. 단일 열이니까 ~~
UITableView는 cell만 커스텀 가능하고, 섹션 배경같은 건 커스텀이 안 돼.
그래서 니가 그냥 기본 list 같은 것만 구현하길 원하고 배경도 그냥 단색같은거면 UITableView 써.
근데 커스텀 inset이나, 섹션별로 border같은게 있으면 UICollectionView를 써.
```

section 별 커스텀 디자인이 있느냐 없느냐에 따라서도 결정할 수 있을 것 같다.

```
A4.
개인적으로 나는 UITableView가 할 수 있는 거의 모든 걸 UICollectionView가 할 수 있다고 생각해.
쓰기야 조금 더 복잡하지만,
기획자가 나중에 요구사항을 변경할 것을 대비해서 그냥 UICollectionView를 TableView로 쓰는 걸 추천해 ~
```

디자인이나 요구사항의 변경을 대비해 미리 UICollectionView를 쓰는 사람도 있었다.

### WWDC20

애플은 WWDC 2020에서 `UICollectionView`에 목록 레이아웃을 도입해 활용성을 한 단계 끌어올렸다.  
iOS14에서 UICollectionView에 추가된 새 기능과 API를 사용하면,  
UITableView같은 list를 구현하기 굉장히 쉬워지고, 그냥 확실히 UITableView를 쓸 필요가 없어진다!

https://swiftsenpai.com/development/uicollectionview-list-basic/

https://www.hackingwithswift.com/forums/ios/using-only-uicollectionview-instead-of-uitableview/2150

