---
layout: post
title: "홈 gradient 실습 1" 
date: 2020-12-30
category: read 
excerpt: ""

---

# 홈 gradient 실습 1

홈을 어떤 식으로 구현할까 고민하고 있었는데,, 그냥 시간 있을 때 다 구현해보고 결정하기로 했다.  
오브제 배치 문제로 고민을 많이 해봤는데,, 두 가지 후보 중 후보 1은 아래와 같다.

```
## 1. tableview만 사용(투명), gradient는 뒤에서 animate되게끔...

장점
- tableview cell 개수에 따라 gradient animate 가능할 듯?
- 코드 깔끔 왜냐면 애초에 UITableView가 UIScollView 상속받은 애라서 ScollView 또 안 써도 됨
- 간지남.

단점
- 근데 물방울이 없어도 모든 단계의 배경은 보여야 하지 않나
    - 단계를 section으로 나누면 되지 않음? ㄷㄷ 미친 대박 나 천잰가바;;
- 스크롤 중에 오브제 배치 우짤.. ㄱ-
```

함 해보기로.. 그리고 새로 안 사실들을 정리할 것임..



* scroll에 따라 animation 주기
  * 가능
  * 근데 이걸로 gradient가 스크롤 되는 느낌을 어떻게 주냐?

* section 별로 배경 그라디언트 바뀌게 하기
  * 가능
  * 근데 gradient가 뚝 뚝 하고 바뀌어버림. gradient가 스크롤 되는게 아니라... ㄱ-;;

* **section별로 배경 그라디언트 지정하기 !!!!!!!!!!**
  * 이게 답이였음 ~~!! 야호야호 실습 2 안 해도 될듯허다 ㅋ
  * tableview는 투명하게 미리 설정해줘야 함



### xib 만들기

![image](https://user-images.githubusercontent.com/28949235/103359789-dffbcd00-4afb-11eb-8e1d-3c3f630ef80a.png)

어차피 테스트용이니까 머.. 대충..

### main storyboard에 tablview 만들기

> 걍 꽉 채워서 .. 상하좌우 0 0 0 0 to SuperView

```swift
@IBOutlet weak var tableView: UITableView!
```

그리고 나중에 통신으로 받아올 각 section(단계) 별 row(bubble)의 개수 배열 선언

```swift
var numberOfRowsInSection = [2, 1, 1, 4, 1, 1, 1]
```



**UITableViewDataSource** : 특별한 건 딱히 없음 ...

```swift
extension ViewController: UITableViewDataSource {
    func numberOfSections(in tableView: UITableView) -> Int {
        return 7
    }
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return numberOfRowsInSection[section]
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        if let cell = tableView.dequeueReusableCell(withIdentifier: "BubbleTableViewCell") as? BubbleTableViewCell {
            return cell
        }
        return UITableViewCell()
    }
    
    func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
        return 150
    }
}
```



**UITableViewDelegate** 

```swift
extension ViewController: UITableViewDelegate {
    func tableView(_ tableView: UITableView, willDisplay cell: UITableViewCell, forRowAt indexPath: IndexPath) {
        cell.backgroundColor = UIColor.clear
    }
    
func tableView(_ tableView: UITableView, didEndDisplaying cell: UITableViewCell, forRowAt indexPath: IndexPath) { //ㅋㅋㅋㅋㅋ
        DispatchQueue.main.async {
                for index in 0 ..< tableView.visibleCells.count {
                    let zPosition = CGFloat(tableView.visibleCells.count - index)
                    tableView.visibleCells[index].layer.zPosition = zPosition
                }
            }
    }
}
```



**extension UIImage - CAGradientLayer를 UIImage로 바꿔주는 함수**

```swift
extension UIImage {
    static func gradientImageWithBounds(bounds: CGRect, colors: [CGColor]) -> UIImage {
        let gradientLayer = CAGradientLayer()
        gradientLayer.frame = bounds
        gradientLayer.colors = colors
        
        UIGraphicsBeginImageContext(gradientLayer.bounds.size)
        gradientLayer.render(in: UIGraphicsGetCurrentContext()!)
        let image = UIGraphicsGetImageFromCurrentImageContext()
        UIGraphicsEndImageContext()
        return image!
    }
}
```



## Gradients

```swift
    var gradientLayer: CAGradientLayer!
    var colorSets = [[CGColor]]()
    var currentColorSet: Int!
```

```swift
    func createColorSets() { //viewDidLoad 에서 호출
        colorSets.append([UIColor.red.cgColor, UIColor.orange.cgColor]) //1단계
        colorSets.append([UIColor.orange.cgColor, UIColor.yellow.cgColor]) //2단계
        colorSets.append([UIColor.yellow.cgColor, UIColor.green.cgColor]) //3단계
        colorSets.append([UIColor.green.cgColor, UIColor.blue.cgColor]) //4단계
        colorSets.append([UIColor.blue.cgColor, UIColor.gray.cgColor]) //5단계
        colorSets.append([UIColor.gray.cgColor, UIColor.purple.cgColor]) //6단계
        colorSets.append([UIColor.purple.cgColor, UIColor.red.cgColor]) //7단계
     
        currentColorSet = 0
    }
```

```swift
    // MARK: - viewDidAppear
    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(false)
        
        for sectionIndex in 0..<tableView.numberOfSections {
            let frame = tableView.rect(forSection: sectionIndex)
            let view = UIView(frame: frame)
            let gradientView = UIView(frame: frame)
            let imgView = UIImageView(frame: view.bounds)
            
            currentColorSet = sectionIndex
            gradientLayer = CAGradientLayer()
            gradientLayer.frame = gradientView.frame
            gradientLayer.colors = colorSets[currentColorSet]
            
            let image = UIImage.gradientImageWithBounds(bounds: frame , colors: colorSets[sectionIndex])
            imgView.image = image
            
            view.addSubview(imgView)
            
            tableView.addSubview(view)
            tableView.sendSubviewToBack(view)
        }
        
    }
```



으아아아 !!! 해냈당

