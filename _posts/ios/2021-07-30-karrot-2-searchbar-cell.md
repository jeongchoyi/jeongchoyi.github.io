---
layout: post
title: "당근마켓 iOS 클론코딩 - 2. 검색 바 cell, dynamic width" 
date: 2021-07-30
category: read 
excerpt: ""

---

# 당근마켓 iOS 클론코딩 - 2. 검색 바 cell, dynamic width

<img src="https://user-images.githubusercontent.com/28949235/127641665-84dda851-01d8-493c-839e-4781c1201cc8.jpeg" alt="IMG_928CE62B70C4-1" width=400 />

오늘 클론할 내용!

### 서론

전체 뷰를 설계할 때, collectionView로 구현하기로 결정했기 때문에  
이번 cell은 UICollectionViewCell로 작성할 것이다.



### 검색바

검색바를 클릭하면 SearchController 뷰로 이동하기 때문에, 사실상 버튼 역할이다.  
UIView 내부에 아이콘과 label을 배치한 후 gesture recognizer또는 addTarget을 달아줄 것이다.

![image](https://user-images.githubusercontent.com/28949235/127642705-c4ddacf7-1604-475e-b8ca-7cf728d08170.png)

요렇게만 배치 후에 코드로  cornerRadius 처리 해 준다.  
cornerRadius는 굉장히 많이 사용하기 때문에,  
나는 UIView Extension에 만들어 놓고 사용한다.

```swift
extension UIView {
    
    func makeRounded(radius: CGFloat) {
        self.clipsToBounds = true
        self.layer.cornerRadius = radius
    }
    
    func makeRoundedWithBorder(radius: CGFloat, color: CGColor) {
        makeRounded(radius: radius)
        self.layer.borderWidth = 1
        self.layer.borderColor = color
    }
  
}
```

기본적으로 이렇게 두 가지 (테두리 있는 것과 없는 것)를 만들어 놓는다.

```swift
searchBarBgView.makeRounded(radius: 5)
```

그럼 이렇게 간단하게 사용할 수 있다.



### 추천 검색어 collection view

<img src="https://user-images.githubusercontent.com/28949235/127643454-232e1598-ec6a-4dc0-9b6a-60aac1e0230a.jpeg" alt="IMG_FB040356E707-1" width=400 />

스크롤 해 보면 범위를 알 수 있다. 스크린의 끝~~ 까지 셀이 이동하므로 collection view를 배치할 때 고려해서 넣어준다.

![image](https://user-images.githubusercontent.com/28949235/127644096-bc2d4d55-b7c2-4493-8448-bc476a5557eb.png)

이렇게! 그리고 그 collection view 안에 들어갈 cell xib 파일을 만들어준다

![image](https://user-images.githubusercontent.com/28949235/127644543-c596073c-735a-4ef2-a2b6-69589ef90c56.png)

요렇게 만들어 줬는데... label과 view의 상하좌우 최소 거리를 보장하기 위해 constraint를 잡아줬다.   
얘도 마찬가지로 코드로 corder radius, border 처리를 해 준다.

그리고 delegate 위임, 기본 stub들 구현.. 등등 기본적인 것들을 해 주면  

![image](https://user-images.githubusercontent.com/28949235/127649355-a78ceb07-f81c-4a1e-93d9-66fedc585719.png)

```swift
// MARK: - UICollectionViewDelegateFlowLayout

extension SearchBarCollectionViewCell: UICollectionViewDelegateFlowLayout {
    
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, insetForSectionAt section: Int) -> UIEdgeInsets {
        return UIEdgeInsets(top: 0, left: 20, bottom: 0, right: 0)
    }
    
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        return CGSize(width: 100, height: 30)
    }
    
}

// MARK: - UICollectionViewDataSource

extension SearchBarCollectionViewCell: UICollectionViewDataSource {
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return keywords.count
    }
    
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: Const.Xib.Identifier.searchKeywordCollectionViewCell, for: indexPath) as? SearchKeywordCollectionViewCell else {
            return UICollectionViewCell()
        }
        cell.setCell(keyword: keywords[indexPath.row])
        return cell
    }
    
}
```



![image](https://user-images.githubusercontent.com/28949235/127649204-e70e95a3-c54c-4651-8162-03a24ba2232a.png)

여기까지 된다.

## dynamic width

```swift
let keywords: [String] = ["정수기", "월세", "딸기", "왁싱", "귤", "쌀", "조명", "미술", "피부관리"]
```

키워드들에 따라 cell의 넓이가 달라져야 한다. dynamic width를 구현하는 데에는 여러가지 방법이 있다



### 더미 UILabel 만든 후 만든 label width 반환하기 (비추)

> 코드로 UILabel을 만들고, label.text에 텍스트를 넣은 후 그 UILabel의 width를 받아 사용하는 방식.  

```swift
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        
    let dummyLabel = UILabel()
    dummyLabel.text = keywords[indexPath.row]
    dummyLabel.sizeToFit()
        
    return CGSize(width: dummyLabel.frame.width + 20, height: 30) // 좌우 여백값 10 * 2를 더해 줌
}
```

![image](https://user-images.githubusercontent.com/28949235/127657220-e00759b9-93e3-4f90-8559-f2a4c084dfca.png)

쉬워 보이지만.. 비추천하는 방법이다.  
이유는 ! ! 

1. 비용이 많이 든다. 
   1. UILabel은 복잡한 객체라서, 필요 없는 경우에도 cell이 다시 보여야 할 순간 매 iteration마다 재생산된다.
2. 오버헤드 해결책이다.
   1. text의 size만 구하면 되는데 전체 UI Object를 만들어버리니까.. 오버헤드다
3. 코드가 더럽다
   1. 위와 같은 이유로 코드가 더러워진다.

쓰지 맙시데~



### NSString의 boundingRect 사용하기

```swift
extension SearchBarCollectionViewCell: UICollectionViewDelegateFlowLayout {

    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        
        let width = self.estimatedFrame(text: keywords[indexPath.row], font: UIFont.systemFont(ofSize: 15)).width
        
        return CGSize(width: width + 20, height: 30)
    }
    
    func estimatedFrame(text: String, font: UIFont) -> CGRect {
        let size = CGSize(width: 200, height: 1000) // temporary size
        let options = NSStringDrawingOptions.usesFontLeading.union(.usesLineFragmentOrigin)
        return NSString(string: text).boundingRect(with: size,
                                                   options: options,
                                                   attributes: [NSAttributedString.Key.font: font],
                                                   context: nil)
    }
    
}
```

이건 비어있다 개발할 때 썼던 건데.. 그냥 구글링 해서 나오는 걸 사용했던 걸로 기억한다.  
아래 방법과 비교해보면 굉~장히 복잡.. ^^ dummy size를 만드는 것도 뭔가 별로다.

![image](https://user-images.githubusercontent.com/28949235/127658145-0abd2d2e-89a0-43b5-b59f-d383a6b37d69.png)

### NSString의 Size 사용하기

![image](https://user-images.githubusercontent.com/28949235/127657970-b0d7bf93-1c2f-4ec6-9765-fc1518132670.png)

NSString에는 size라는 메소드가 있다

```swift
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        
    let itemSize = keywords[indexPath.row].size(withAttributes: [
    		NSAttributedString.Key.font : UIFont.boldSystemFont(ofSize: 15)
    ])
        
    return CGSize(width: itemSize.width + 20, height: 30)
}
```

요렇게 String의 size를 받아 온 후, `itemSize.width`로 사용하면 된다.  
Attributes도 설정할 수 있어서, 폰트의 크기나 weight에 따라 size를 받아올 수 있다.

![image](https://user-images.githubusercontent.com/28949235/127658184-83bdf3af-87a9-4be3-8613-62f0a513ad00.png)

간단하고 깔끔 ~!



그후 padding값 등을 수정해줬다.  
height값도 지정이므로  
![image](https://user-images.githubusercontent.com/28949235/127659355-5f7d3728-d888-47e7-8b61-ccf2044c856b.png)

label을 vertical center로 맞춰줬다.

## 클론 완료

클론할 것

<img src="https://user-images.githubusercontent.com/28949235/127643454-232e1598-ec6a-4dc0-9b6a-60aac1e0230a.jpeg" alt="IMG_FB040356E707-1" width=400 />

클론한 것

![image](https://user-images.githubusercontent.com/28949235/127659501-4ec0d989-0b57-4309-9413-e5a47322a10b.png)

