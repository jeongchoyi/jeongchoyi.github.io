---
layout: post
title: "UICollectionView 페이징처리 셀 단위로 하기" 
date: 2020-11-25
category: read 
excerpt: ""

---

# UICollectionView carousel effect

![Screen Shot 2020-11-28 at 11 15 22 PM](https://user-images.githubusercontent.com/28949235/100517684-d109be00-31cf-11eb-8a3c-a1f4ff30466c.png)

여기서 `Paging Enabled`에 체크하면 우선 paging이 되긴 하는데..  
1 page의 단위가 하나의 셀이 아니라 뷰 단위가 된다.

![Screen Shot 2020-11-28 at 11 20 40 PM](https://user-images.githubusercontent.com/28949235/100517747-71f87900-31d0-11eb-9e3d-7861413f4933.png)

셀을 이런 식으로 만들었다면, 처음에는 예쁘게 옆에도 살짝 보이고 잘 나오지만  
페이지를 넘기면 넘길수록 난리가 난다.

![image](https://user-images.githubusercontent.com/28949235/100517794-b2f08d80-31d0-11eb-952f-8078f544fe47.png)<img src="https://user-images.githubusercontent.com/28949235/100517808-d1568900-31d0-11eb-9210-63812c043e4e.jpeg" alt="IMG_B7A2F7F9C958-1" width=300 />

> 대참사가 일어남... (오른쪽이 내가 하고싶던 것)

요건 carousel effect라고 검색하면 많이 나오는데,  
[여기](https://eunjin3786.tistory.com/203) 다양한 상황에서의 carousel effect 구현법이 있으니 참고하기..

내가 한 과정을 정리해보자!

### 기본 설정

```swift
collectionView.decelerationRate = .fast
collectionView.isPagingEnabled = false
```

위에서 체크했던 `Paging Enabled`도 체크 해제해준다.

```swift
    func scrollViewWillEndDragging(_ scrollView: UIScrollView, withVelocity velocity: CGPoint, targetContentOffset: UnsafeMutablePointer<CGPoint>) {
        guard let layout = self.newsCollectionView.collectionViewLayout as? UICollectionViewFlowLayout else { return }
        
        // CollectionView Item Size
        let cellWidthIncludingSpacing = layout.itemSize.width + layout.minimumLineSpacing
        
        // 이동한 x좌표 값과 item의 크기를 비교 후 페이징 값 설정
        let estimatedIndex = scrollView.contentOffset.x / cellWidthIncludingSpacing
        let index: Int
        
        // 스크롤 방향 체크
        // item 절반 사이즈 만큼 스크롤로 판단하여 올림, 내림 처리
        if velocity.x > 0 {
            index = Int(ceil(estimatedIndex))
        } else if velocity.x < 0 {
            index = Int(floor(estimatedIndex))
        } else {
            index = Int(round(estimatedIndex))
        }
        // 위 코드를 통해 페이징 될 좌표 값을 targetContentOffset에 대입
        targetContentOffset.pointee = CGPoint(x: CGFloat(index) * cellWidthIncludingSpacing, y: 0)
    }
```

(xib 셀 내 좌우여백이 없거나 동일해야 함!)

난 셀 간격을 셀에서 주지 않고 `awakeFromNib()`에서 줬다

```swift
let flowLayout = UICollectionViewFlowLayout()
flowLayout.scrollDirection = .horizontal
flowLayout.itemSize = CGSize(width: 375, height: 250)
flowLayout.minimumLineSpacing = 10
```

이런식...



![image](https://user-images.githubusercontent.com/28949235/100518944-2053ec80-31d8-11eb-9532-af0c6df009f2.png)

아싸