---
layout: post
title: "UICollectionView 무한 스크롤 (infinite carousel) 야매로 구현하기" 
date: 2020-11-29
category: read 
excerpt: ""

---

# UICollectionView 무한 스크롤 (infinite carousel) 야매로 구현하기

### 데이터 형식

```swift
var banners:[String] = ["plan", "server", "design", "ios", "android", "web"]
```

데이터가 이렇게 존재할 때,

### UICollectionView DataSource

**numberOfItemsInSection**

```swift
func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
    return 1000 //banners.count 대신 큰 수를 return
}
```

설마 1000번 스크롤 하겠어? 라는 생각으로 큰 수를 잡아줌

**cellForItemAt**

```swift
func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
   //let itemToShow = banners[indexPath.row % banners.count]
   guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "PartBannerCell", for: indexPath) as? PartBannerCell else {
        return UICollectionViewCell()
   }
   cell.setImage(imageName: banners[indexPath.row % banners.count])
   return cell
}
```

`indexPath.row % banners.count` 번째 data에 맞는 사진을 띄워주면 된다.  
(data에 사진 말고 다른 게 필요하면 똑같이 하면 됨)

해당 row번호를 banners 배열의 크기로 나눈 값의 나머지에 따른 data라서,  
data는 결국 banners 배열 내의 값으로 뿌려지게 된다.



이렇게 까지 하면 오른쪽 방향으로 1000까지 data가 반복되며 스크롤이 되는데,  
양쪽으로 스크롤이 가능하게 하기 위해 collectionview에서의 시작점을 조절해준다.

### ViewDidLoad

```swift
 self.partCollectionView.scrollToItem(at:IndexPath(item: 500, section: 0), at: .right, animated: false)
```

500번째 item에서 시작



이해를 위한 이미지

![Untitled-1](https://user-images.githubusercontent.com/28949235/100536058-6acd7b80-3261-11eb-8469-bce05397fee0.png)

`var banners:[String] = ["plan", "server", "design", "ios", "android", "web"] ` 이므로

`banners[2] = "design"`

