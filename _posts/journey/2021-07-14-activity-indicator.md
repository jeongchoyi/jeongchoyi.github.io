---
layout: post
title: "Activity Indicator 추가하기" 
date: 2021-07-14
category: read 
excerpt: ""

---

# Activity Indicator 추가하기

```swift
    private lazy var activityIndicator: UIActivityIndicatorView = {
        let activityIndicator = UIActivityIndicatorView()
        activityIndicator.frame = CGRect(x: 0, y: 0, width: 50, height: 50)
        activityIndicator.center = CGPoint(x: self.view.center.x, y: self.view.center.y - self.topbarHeight)
        activityIndicator.hidesWhenStopped = false
        activityIndicator.style = UIActivityIndicatorView.Style.medium
        activityIndicator.startAnimating()
        return activityIndicator
    }()
```



떼고, 붙이는 함수도 만들어주고

```swift
    private func attachActivityIndicator() {
        backgroundView.backgroundColor = UIColor.white
        self.view.addSubview(backgroundView)
        self.view.addSubview(self.activityIndicator)
    }
    
    private func detachActivityIndicator() {
        if self.activityIndicator.isAnimating {
            self.activityIndicator.stopAnimating()
        }
        self.backgroundView.removeFromSuperview()
        self.activityIndicator.removeFromSuperview()
    }
```

![image](https://user-images.githubusercontent.com/28949235/125582205-d01d918c-6dd0-414f-9a7d-88e935a16cd8.png)

통신 시작할때 붙이고

![image](https://user-images.githubusercontent.com/28949235/125582273-a68f2fa9-ccf8-4039-9edb-f60a85675ae4.png)

통신 끝나고 호출되는 함수에서 떼주면 완성!
