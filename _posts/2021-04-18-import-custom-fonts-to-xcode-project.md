---
layout: post
title: "커스텀 폰트 엑스코드 프로젝트에 추가하기" 
date: 2021-04-18
category: read 
excerpt: ""

---

# 커스텀 폰트 엑스코드 프로젝트에 추가하기

#### 1. 폰트를 다운받는다

<img src="/Users/choyi/Library/Application Support/typora-user-images/image-20210418120851001.png" alt="image-20210418120851001" style="zoom:50%;" />

ttf, otf 상관 없다!

#### 2. 프로젝트에 폰트 파일을 추가해준다

<img src="https://user-images.githubusercontent.com/28949235/115132827-add89180-a03e-11eb-9e6b-361258aacbca.png" alt="image" style="zoom:50%;" />



 <img src="https://user-images.githubusercontent.com/28949235/115132874-fbed9500-a03e-11eb-8848-4daa14429228.png" alt="image" style="zoom:50%;" />

`Copy items if needed`에 체크가 되어있는지 확인하고,  
target도 잘 체크 되어있나 확인하고  `Add`를 해준다.

<img src="https://user-images.githubusercontent.com/28949235/115132899-22133500-a03f-11eb-8b61-325fb7b858fe.png" alt="image" style="zoom:50%;" /> 

추가된 모습!

#### 3. Info.plist에 폰트 파일 명시

<img src="https://user-images.githubusercontent.com/28949235/115133237-9b138c00-a041-11eb-9af9-fd42919afd88.png" alt="image" style="zoom:50%;" />

Info.plist에 `Fonts provided by application` 항목을 만들고,  
폰트 파일명을 적어주는데 **확장자 포함**해서 적어준다.  
추가한 폰트 파일만큼 추가해서 적어주면 된다 (Array)

#### 4. 사용하기

```swift
beerAwardLabel.font = UIFont(name: "GmarketSansBold", size: 26)
```

`font` 프로퍼티 사용 ! UIFont의 name 에는 확장자를 뗀 폰트 이름만 써주면 된다.

<img src="https://user-images.githubusercontent.com/28949235/115134783-31e64580-a04e-11eb-88d4-f02ae045b530.png" alt="image" style="zoom:50%;" />

잘 적용된 모습 ~!