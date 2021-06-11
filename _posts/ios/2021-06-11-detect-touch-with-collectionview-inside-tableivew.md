---
layout: post
title: "tableview cell 안의 collectionview - 2. 터치 인식하기" 
date: 2021-06-11
category: read 
excerpt: ""

---

# tableview cell 안의 collectionview - 2. 터치 인식하기

[ tableview cell 안의 collectionview: 뷰 그리고 임시 데이터 넣기 ](https://iamcho2.github.io/2021/05/17/pass-data-with-collectionview-inside-tableview)   
전 포스팅에 이어서... 이제는 각 collectionView cell을 클릭한 것에 따른 액션을 줄 시간!

![image](https://user-images.githubusercontent.com/28949235/121699164-a945cc00-cb09-11eb-9735-abe9e9e0ec96.png)

오늘은 요 뷰로 작업~~~~ 🍺

생각을 먼저 해보면,  
몇번째 index에 있는 collection view cell이 선택되었는지 tableview cell이 알아야 하고...  
그걸 main table view까지 가져와야 하는 것이다 !!  안쪽부터 window쪽으로 쭉쭉 몇번째인지 가져와야 되는 것.

이를 위해 **protocol**을 만들어야 한다.

## protocol 만들기

TableViewCell 에다가 protocol을 만들어준다.  
안에 함수를 만들건데, CollectionViewCell이랑 index(position), 그리고 어떤 TableViewCell이 터치되었는지를 반환할 것 !

![image](https://user-images.githubusercontent.com/28949235/121699859-49035a00-cb0a-11eb-92f7-60738d839e67.png)

```swift
protocol CollectionViewCellDelegate: AnyObject {
    func collectionView(collectionviewcell: MainCollectionViewCell?, index: Int, didTappedInTableViewCell: MainTableViewCell)
}

```

**MainTableViewCell** class에 변수도 선언해준다.

```swift
weak var cellDelegate: CollectionViewCellDelegate?
```

그 테이블 뷰 셀 내 **UICollectionViewDataSource** Extension에 `didSelectItemAt()` 함수를 구현해준다.

```swift
func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
    let cell = collectionView.cellForItem(at: indexPath) as? MainCollectionViewCell
    self.cellDelegate?.collectionView(collectionviewcell: cell, index: indexPath.item, didTappedInTableViewCell: self)
}
```

사용자가 cell을 터치 할 때 마다, 요 함수가 호출된다.  
그리고, protocol에 의해 데이터가 전달되는 것!



TableView 내의 데이터를 전달받기 위해서는,  
VC에 있는 TableView로 가서 CellForRowAt 내부에다가 cell의 delegate를 설정해줘야 한다.

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        if let cell = tableView.dequeueReusableCell(withIdentifier: identifier 내용 ) as? TableViewCell {
            // ...
            
            // 여기에 써주면 됨
            cell.cellDelegate = self
            
            // ...
            return cell
        }
        return UITableViewCell()
    }
```

guard let, 또는 if let으로 만든 cell을 return 하기 전에 `cell.cellDelegate = self` 를 써주면 된다.

![image](https://user-images.githubusercontent.com/28949235/121701245-a9df6200-cb0b-11eb-9bb9-3c96d893bd4b.png)

그럼 요런 빨간줄이 뜨는데,  
VC에다가 **CollectionViewCellDelegate** extension을 만들고  
아까 우리가 프로토콜 내에 시그니처만 만들어준 함수를 구현해 놓으면 된다.

```swift
extension MainViewController: CollectionViewCellDelegate {
    func collectionView(collectionviewcell: MainCollectionViewCell?, index: Int, didTappedInTableViewCell: MainTableViewCell) {

            print("어쩌구저쩌구")
    }
}
```

요 함수 내에서 분기처리를 해 줄수도 있다~!

![Screen-Recording-2021-06-11-at-11 41 53-PM](https://user-images.githubusercontent.com/28949235/121704997-39d2db00-cb0f-11eb-8974-ff12f9a89ece.gif)

끝!