---
layout: post
title: "tag box collection view cell 만들기" 
date: 2021-03-29
category: read 
excerpt: ""

---

# tag box collection view cell 만들기

![image](https://user-images.githubusercontent.com/28949235/112790618-be7a9500-909a-11eb-87bc-d7f1454e39a7.png)

여기서 사용되는 collection view cell을 만들건데,  
스타일, 향 뷰에서 재사용되기 때문에 SelectedFilterCollectionViewCell이라고 네이밍 했다.

xib파일 만들고... 클래스 연결 하고..

![image](https://user-images.githubusercontent.com/28949235/112790691-e10cae00-909a-11eb-8cfa-a675b0cacfc3.png)

SF Symbol 사용하고..

![image](https://user-images.githubusercontent.com/28949235/113392255-c8203780-93cf-11eb-9681-1d7dfb123d1a.png)

### rounded corner border

```swift
        bgView.clipsToBounds = true
        bgView.layer.cornerRadius = 15
        bgView.layer.borderWidth = 1
        bgView.layer.borderColor = UIColor.Black.cgColor
```

### register xib

```swift
selectedStyleCollectionView.register(UINib(nibName: Const.Xib.Name.selectedFilterCollectionViewCell, bundle: nil), forCellWithReuseIdentifier: Const.Xib.Identifier.selectedFilterCollectionViewCell)
```

### Dynamic cell width

```swift
 func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        
        if collectionView == selectedStyleCollectionView {
            let titles = ["전체보기", "Bourbon County Stout", "Bourbon County Stout"]
            
            let width = self.estimatedFrame(text: titles[indexPath.row], font: UIFont.systemFont(ofSize: 10)).width
            return CGSize(width: width, height: 50.0)
        }
        return CGSize(width: 5, height: 5)
    }
```

```swift
    func estimatedFrame(text: String, font: UIFont) -> CGRect {
        let size = CGSize(width: 200, height: 1000) // temporary size
        let options = NSStringDrawingOptions.usesFontLeading.union(.usesLineFragmentOrigin)
        return NSString(string: text).boundingRect(with: size,
                                                   options: options,
                                                   attributes: [NSAttributedString.Key.font: font],
                                                   context: nil)
    }
```

다른 collectionview 때문에 `sizeForItemAt`을 사용해야 해서,  
이렇게 `estimatedFrame()`을 사용해서 유동적으로 넓이값을 설정해준다.

