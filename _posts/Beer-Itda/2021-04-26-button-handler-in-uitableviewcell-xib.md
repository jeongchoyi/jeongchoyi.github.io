---
layout: post
title: "UITableVIewCell의 버튼 트리거 다루기" 
date: 2021-04-26
category: read 
excerpt: ""

---

# UITableVIewCell의 버튼 트리거 다루기

![image](https://user-images.githubusercontent.com/28949235/116054812-0c44e600-a6b7-11eb-89c8-8b7bca883876.png)

먼저 tableview xib에서 버튼을 @IBOutlet 연결 해 준다.

해당 tableview가 있는 VC 파일로 가서,  
`cellForRowAt` 함수 내부에서 버튼에 태그를 달아준다.

```swift
cell.moreButton.tag = indexPath.row
```

버튼에 addTarget 함수로 트리거를 연결해준다.

```swift
cell.moreButton.addTarget(self, action: #selector(pushToBeerAllViewController), for: .touchUpInside)
```

그리고 @objc 함수도 만들어준다!

```swift
    @objc func pushToBeerAllViewController(sender: UIButton) {
        
        let beerAllStoryboard = UIStoryboard(name: Const.Storyboard.Name.beerAll, bundle: nil)
        guard let beerAllViewController = beerAllStoryboard.instantiateViewController(withIdentifier: Const.ViewController.Identifier.beerAll) as? BeerAllViewController else {
            return
        }
        
        self.navigationController?.pushViewController(beerAllViewController, animated: true)
    }

```

해당 함수 내에서 button에 달아준 tag로 분기처리를 해줄 수도 있다.

```swift
if sender.tag == 0 {
    print("스타일")
}
```



