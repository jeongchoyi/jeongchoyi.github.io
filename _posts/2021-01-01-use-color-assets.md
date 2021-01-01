---
layout: post
title: "Color Asset 사용하기" 
date: 2021-01-01
category: read 
excerpt: ""

---

# Color Asset 사용하기

간단합니당

### 만들기

![image](https://user-images.githubusercontent.com/28949235/103439717-a6a19980-4c82-11eb-9539-05a3bd912a9f.png)

이건 내가 만들어 뒀으니 열분은 안 만들어도 됩니도 !!! `Colors.xcassets` 입니다

### 색상 추가하기

![image](https://user-images.githubusercontent.com/28949235/103439754-0304b900-4c83-11eb-8971-65d2ac237c7b.png)

`+` 버튼 누르고 `Color Set` 추가를 해줍니다

![image](https://user-images.githubusercontent.com/28949235/103439758-08fa9a00-4c83-11eb-9285-cf8dc36d9216.png)

그러면 이렇게 색상이 추가되는데, Any Appearance에는 기본 색상을,  
Dark Appearance에는 해당 색이 다크모드에서 나타날 색상을 넣어주면 됩니다 ~!  
당연히 이름도 적절히 바꿔줘야겠져 보통 UpperCamelCase를 쓰는 것 같더라고요,,

![image](https://user-images.githubusercontent.com/28949235/103439774-3d6e5600-4c83-11eb-92eb-e4fe39bb4e67.png)

색상 지정을 원하는 Appearance를 클릭한 후 오른쪽 패널에

![image](https://user-images.githubusercontent.com/28949235/103439777-4101dd00-4c83-11eb-9503-d4dd102b20de.png)

여기서 조절해주면 됩니당 Show Color Panel 버튼을 누르면 

![image](https://user-images.githubusercontent.com/28949235/103439780-4bbc7200-4c83-11eb-8c1d-c1c51f9bad69.png)

hex color로도 입력할 수 있어요 ~.~

### 사용하기

코드에서 사용하는 법!  
`UIColor`의 `init` 함수를 사용합니다.  

```swift
init?(named name: String)
```

`UIColor(named: "BabyPink")` 이렇게여. 예시를 보자면,,

![image](https://user-images.githubusercontent.com/28949235/103439856-f92f8580-4c83-11eb-8e12-ccdc9e778341.png)

이렇게 Color Asset에 색상들을 추가해줬다면!

전 원래 요런 함수를 만들어 놨는데 말이져

```swift
func createColorSets() {
    colorSets.append([UIColor(hex:0x618AC0).cgColor, UIColor(hex:0x3C7A87).cgColor])
    colorSets.append([UIColor(hex:0x3C7A87).cgColor, UIColor(hex:0x37938D).cgColor])
    colorSets.append([UIColor(hex:0x37938D).cgColor, UIColor(hex:0x277473).cgColor])
    colorSets.append([UIColor(hex:0x277473).cgColor, UIColor(hex:0x2C607A).cgColor])
    colorSets.append([UIColor(hex:0x2C607A).cgColor, UIColor(hex:0x194576).cgColor])
    colorSets.append([UIColor(hex:0x194576).cgColor, UIColor(hex:0x081A3F).cgColor])
    colorSets.append([UIColor(hex:0x081A3F).cgColor, UIColor(hex:0x051B28).cgColor])
    
    currentColorSet = 0
}
```

저 한 줄을 이렇게 바꿀 수 있겠죠~! (hex extension 함수 사용한거임)

```swift
colorSets.append([UIColor(named: "Gradient1")!.cgColor, UIColor(named: "Gradient2")!.cgColor])
```

근데 이건 좀 약간 위험해보이고(?) 암튼.. 아래처럼도 쓸 수 있어요.



UIColor+Extension.swift 파일에 아래와 같은 static 상수들을 선언 해 놓는거져

```swift
extension UIColor {
    static let Gradient1: UIColor = UIColor(named: "Gradient1")!
}
```

모 이렇게? 그리고 사용할 땐 아래처럼 자동완성도 뜨고요~

![image](https://user-images.githubusercontent.com/28949235/103439995-61cb3200-4c85-11eb-9317-639b05fceddf.png)

```swift
colorSets.append([UIColor.Gradient1.cgColor, UIColor.Gradient2.cgColor])
```

이런 식이 되겠죠 ~! xcode에서 보면

![image](https://user-images.githubusercontent.com/28949235/103440035-b8d10700-4c85-11eb-923a-3de8c201c97a.png)

위의 두 줄이 젤 깔끔 ~^___^

