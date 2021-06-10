---
layout: post
title: "향, 스타일 건너뛰기에 따른 메인 뷰 table view cell 분기처리" 
date: 2021-06-10
category: read 
excerpt: ""

---

# 향, 스타일 건너뛰기에 따른 메인 뷰 table view cell 분기처리

> Enum을 사용한 분기처리!

![IMG_22E21E7A3AE3-1](https://user-images.githubusercontent.com/28949235/121480569-2be56300-ca06-11eb-8bd7-3e97d442cbbc.jpeg)



요렇게 설계해놓고... Enum을 먼저 만들어보자

### Enum 만들기

Enum은 행동이 trigger되는 뷰가 아닌 다음 뷰에 작성한다.  
( Style 뷰에서 skip 버튼을 누르지만, Scent뷰에 Style 뷰 skip 관련 Enum을 작성한다)

```swift
    enum IsStyleSkipped: Int {
        case unskip = 0, skip
    }
    var isStyleSkipped: IsStyleSkipped?
```

네이밍... 괜찮은지 모르겠지만... 아무튼 이 것을 Style의 다음 뷰인 Scent 뷰에 작성해주고,  
다시 Style 뷰로 돌아와서

### Style 뷰 skip 여부 전달하기

```swift
    @IBAction func touchSelectButton(_ sender: Any) {
        pushToScentViewController(isSkip: false)
    }

    @IBAction func touchSkipButton(_ sender: Any) {
        pushToScentViewController(isSkip: true)
    }
```

![image](https://user-images.githubusercontent.com/28949235/121482219-e3c74000-ca07-11eb-917a-9a31d831c037.png)

if문으로 이렇게 분기처리 해 주면 된다.



### Tabbar VC에 Enum 만들기

Style, Scent 뷰의 모든 skip 여부를 전달받을 Tabbar VC에 Enum을 만든다.

```swift
    enum IsStyleScentSkipped: Int {
        // style, scent 순서
        case unskipUnskip = 0, unskipSkip, skipUnskip, skipSkip
    }
    
    var isStyleScentSkipped: IsStyleScentSkipped?
```

와 네이밍 진짜 맘에 안든다 ㅎㅎ

### Scent 뷰 skip 여부 전달하기

```swift
    @IBAction func touchSelectButton(_ sender: Any) {
        pushToMainViewController(isSkip: false)
    }
    
    @IBAction func touchSkipButton(_ sender: Any) {
        pushToMainViewController(isSkip: true)
    }
```

![image](https://user-images.githubusercontent.com/28949235/121484135-e3c83f80-ca09-11eb-8c08-57762499c716.png)

### Tabbar VC에서 Main VC로 Enum 전달하기

Main에도 Tabbar와 똑같이 Enum을 만들어 준다.

그리고 Tabbar에서 Main탭이 선택되었을 때 분기처리를 해 주면 ~~된다.~~ **안 된다.**

![image](https://user-images.githubusercontent.com/28949235/121484649-64873b80-ca0a-11eb-8357-e29643771aaf.png)

이렇게 했으나... 자꾸 Main에선 nil이 넘어왔다.
**당연함.**  
애초에 저 delegate 함수는 최초엔 불려지지도 않고... (탭 변경 후 다시 Main 돌아올 때 호출됨)

Scent -> **Tabbar -> NavigationController -> Main** 니까!!!!!!!!  ㄷㄷ 바보였다

![image](https://user-images.githubusercontent.com/28949235/121487545-26d7e200-ca0d-11eb-841a-c344da4a41db.png)

요것을 tabbarVC의 viewDidLoad에 넣어줘야 Main까지 전달이 잘 된다.

### Main에서 table view cell hidden/unhidden 분기처리 하기

![image](https://user-images.githubusercontent.com/28949235/121490561-1b39ea80-ca10-11eb-9662-2b1a889b406b.png)

cellForRowAt 함수



![image](https://user-images.githubusercontent.com/28949235/121490714-3efd3080-ca10-11eb-9f1e-93800c6d43b1.png)

mainTableViewCell 반환 함수 (나중에 바틀샵Cell 반환 함수도 만들 예정)



![image](https://user-images.githubusercontent.com/28949235/121490657-31e04180-ca10-11eb-992e-33145f78364b.png)

요런거 4개

<img src="https://user-images.githubusercontent.com/28949235/121490853-6227e000-ca10-11eb-9a68-07f26ef1e673.png" alt="image" width=400px />

분기처리 잘 되는 모습..

