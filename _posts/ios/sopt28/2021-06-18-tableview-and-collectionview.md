---
layout: post
title: "테이블뷰와 컬렉션뷰" 
date: 2021-06-18
category: read 
excerpt: ""

---

# SOPT 28기 iOS 3차 세미나

## TableView

### 테이블 뷰 셀 스타일 지정하기

<img src="https://user-images.githubusercontent.com/28949235/122498156-95c6c380-d029-11eb-9abb-94c51ec69145.png" alt="image" width=700px />

### awakeFromNib()

> 객체가 초기화(인스턴스화)된 후 호출되는 메서드 (VC의 viewDidLoad()와 비슷한 개념)  
> awakeFromNib()가 호출되기 전 init(coder: )가 호출됨. (코드로 Cell을 짜게 되면 이 부분을 작성)
>
> init(coder: ) --> frame이나 layer 관련 없는 값들만  
> awakeFromNib() --> frame, layer, @IBOutlet 관련 값 접근 가능

### dynamic cell height

**heightForRowAt에서 height를 automaticDimension으로 지정**

```swift
func tableView(_ tableView: UITableView, heightForRowAt indexPath: indexPath) -> CGFloat {
  return UITableView.automaticDimension
}
```

**추정치 높이 대략적으로 잡아주기**

```swift
tableView.estimatedRowHeight = 100
```

**Xib 등에서 레이아웃 잘 잡아주기 (자동으로 높이를 잘 계산할 수 있도록)**

<img src="https://user-images.githubusercontent.com/28949235/122498685-86944580-d02a-11eb-9fd5-2301b7ab1de3.png" alt="image" width=300px />

### 비슷한 테이블뷰가 여러 VC에서 쓰일 때

> 일일히 DataSource, Delegate를 구현할 필요 없이  
> DataSource, Delegate를 분리해 구현해서 계속 가져다 쓰면 됨

**DataSource 파일**

```swift
import UIKit

class SampleTableDataSource: NSObject, UITableViewDataSource {
  var dataList: [String] = []
  
  // 함수 구현
}
```

**Delegate 파일**

```swift
import UIKit

class SampleTableDelegate: NSObject, UITableViewDelegate {
  // 함수 구현
}
```

**VC에서**

```swift
let sampleSource = SampleTableDataSource()
let sampleDelegate = SampleTableDelegate()
```

```swift
@IBOutlet weak var firstTableVIew: UITableVIew!{
  didSet{
    ...
    firstTableView.delegate = sampleDelegate
    firstTableView.dataSource = sampleSource
    let testNib = UINib(nibName: "TestCell2", bundle: nil)
    firstTableView.register(testNib, forcellReuseIdentifier: "TestCell2")
  }
}
```

## CollectionView

### Estimate Size

> 초기에 셀들의 위치를 임시 배정할 때 쓰임  
> Autolayout 연산을 마치게 되면 그에 따라서 Content Size를 조절하게 되는데,
>
> 우리는 UICollectionVIewDelegateFlowLayout 이라는 프로토콜을 채택해서  
> 코드로 사이즈를 잡아줄 것.
>
> Estimate Size를 열어두고 (None으로 설정하지 않고) 코드로 크기를 작성하면  
> 의도한 사이즈대로 나오지 않을 수 있음 !!

### FlowLayout

<img src="https://user-images.githubusercontent.com/28949235/122499344-ba239f80-d02b-11eb-8299-35239df97618.png" alt="image" width=600px />

> CollectionView는 Delegate와 DataSource를 통해 Cell 자체를 구성하고  
> Cell들의 간격/크기 등을 FlowLayout을 통해 결정하게 됨

**FlowLayout 에서 자주 쓰는 함수들**

* sizeForItemAt : 각 cell들의 크기 (CGSize)
* insetForSectionAt: Cell에서 Content외부에 존재하는 Inset의 크기를 결정 (UIEdgeInsets)
* minimumLineSpacing : Cell들의 위, 아래 간격 지정 (CGFloat)
* minimumInteritemSpacing: Cell들의 좌,우 간격 지정 (CGFloat)



## 복습 자료



이런 뷰를 구현하려면?   
아래는 CollectionVIew인데... CollectionView는 TableView처럼 UIView를 헤더로 올리기 쉽지 않음.  
(가로 길이 때문에 맞추기 까다로움)  
--> **Collection Reusable View** 사용

오브젝트 라이브러리에서 끌어서 VC에 놓고,  
Identifier를 설정하고 아래의 코드로 Supplementary View 구현

```swift
func collectionView(_ collectionView: UIcollectionView, viewForSupplementaryElementOfKind kind: String, at indexPath; IndexPath) -> UICollectionReusableVIew {
  switch kind {
    case UICollectionView.elementKindSectionHeader:
    	let headerView = collectionView.dequeueResuableSupplementaryView(OfKind: kind,
                                    withReuseIdentifier: "reusableView", for: indexPath)
    	return headerView
    
    default: assert(false, "에러")
  }
}
```

<img src="https://user-images.githubusercontent.com/28949235/122500859-8b5af880-d02e-11eb-9dbb-db730a5611be.png" alt="image" width=400px />

> 실제 라인 구현 모습



> SOPT 28기 iOS 파트 3차, 3차 복습 자료 내용 中

