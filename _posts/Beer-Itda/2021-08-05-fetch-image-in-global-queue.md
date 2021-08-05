---

layout: post
title: "이미지를 global queue에서 받지 않으면 일어나는 충격적인 일" 
date: 2021-08-05
category: read 
excerpt: ""

---

## 이미지를 global queue에서 받지 않으면 일어나는 충격적인 일

( 어그로 유튜브 썸네일 느낌으로 )

무슨 일이 일어나냐면...

<img src="https://user-images.githubusercontent.com/28949235/128367063-30b0c544-18d9-45ff-bb76-5a7a85318764.gif" alt="Screen-Recording-2021-08-05-at-11 25 44-PM" width=300px />

이렇게 뚝딱이가 된다. tableview 안에 collectionview를 넣어놓은 모습인데,  
collectionview를 스크롤 할 땐 물론이고, tableview를 스크롤 할 때도 뚝......딱 거리면서 스크롤된다.  
이 문제는 다 이미지를 global queue 에서 받지 않아서 생긴 일..! ㄷ ㄷ

데이터 개수가 많으면 더 난리나겠지요...

```swift
DispatchQueue.global(qos: .background).async {

        let url = URL(string: beer.thumbnailImage)
        // TODO: - url 없을시 에러 처리
        let data = try? Data(contentsOf: url!)
        	DispatchQueue.main.async {
        self.beerImageView.image = UIImage(data: data!)
    }
  
}
```

요렇게, 이미지를 다운로드 받는 건 global queue 에서 수행하고,  
UI에 반영하는 작업은 main queue에서 수행해줘야 한다.  
맨날 main queue만 써보고... global queue 쓴 건 처음이라 정리.. !!



![Screen-Recording-2021-08-05-at-11 24 31-PM](https://user-images.githubusercontent.com/28949235/128367034-4e9c618d-da5d-4407-99c6-475a4a33deaa.gif)


아주 스무스한... glitch 없는 쾌적한 스크롤 ~~

### 덧붙여서

reload()함수는 항상 main thread에서 실행되어야 한다.

```swift
DispatchQueue.main.async {
    self.mainTableView.reloadData()
}
```

