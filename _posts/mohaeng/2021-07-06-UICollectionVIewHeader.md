---
layout: post
title: "UICollectionViewHeader" 
date: 2021-07-06
category: read 
excerpt: ""

---

# UICollectionView Header

> 헷갈리는 거 저장용,,

### 1. UICollectionReusableView Xib 만들기

<img src="https://user-images.githubusercontent.com/28949235/124726268-8fe94180-df48-11eb-9920-b502b7f166e2.png" alt="image" width=400px />

### 2. Xib register하기

```swift
self.medalCollectionView.register(UINib(nibName: Const.Xib.Name.medalCollectionReusableView, bundle: nil), forSupplementaryViewOfKind: UICollectionView.elementKindSectionHeader, withReuseIdentifier: Const.Xib.Identifier.medalCollectionReusableView)
```

cell xib register할 때와는 다르게

```swift
medalCollectionView.register(<#T##viewClass: AnyClass?##AnyClass?#>, forSupplementaryViewOfKind: <#T##String#>, withReuseIdentifier: <#T##String#>)
```

이 메서드를 사용해서 register 해 줘야 한다.  
kind는 `elementKindSectionHeader` 로!

### 3. viewForSupplementaryElementOfKind

```swift
func collectionView(_ collectionView: UICollectionView, viewForSupplementaryElementOfKind kind: String, at indexPath: IndexPath) -> UICollectionReusableView {
                
        if let headerView = collectionView.dequeueReusableSupplementaryView(ofKind: UICollectionView.elementKindSectionHeader, withReuseIdentifier: Const.Xib.Identifier.medalCollectionReusableView, for: indexPath) as? MedalCollectionReusableView {
            
            return headerView
        }
        
        return UICollectionReusableView()
}
```

`viewForSupplementaryElementOfKind` 함수 내에서 headerview를 반환해주는데,  
보통 이 함수 내에서 kind로 switch 문을 통해 분기처리 해준다.

### 4. referenceSizeForHeaderInSection

```swift
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, referenceSizeForHeaderInSection section: Int) -> CGSize {
    return CGSize(width: collectionView.frame.width, height: 300)
}
```

이렇게 height까지 지정해줘야 헤더가 보인다.
