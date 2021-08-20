---
layout: post
title: "FontBlaster 도입하고 폰트 추가하기" 
date: 2021-08-20
category: read 
excerpt: ""


---

# FontBlaster 도입하고 폰트 추가하기

야곰님 블로그 보다가

### FontBlaster

외부 서체(폰트)를 손쉽게 가져와서 활용할 수 있도록 도와주는 라이브러리입니다. 
기본 폰트가 아닌 앱 전용 폰트를 사용한다면 매우 유용하게 사용할 수 있습니다.
https://github.com/ArtSabintsev/FontBlaster

FontBlaster라는 라이브러리를 발견했다.

<img src="https://user-images.githubusercontent.com/28949235/130188634-e007bd7f-b2c2-4cc9-b1ba-84d85990f94b.png" alt="image" style="zoom:50%;" />

지금 만들고 있는 모행이라는 앱은 릴리즈 준비 전 부터 기본 폰트를 거의 쓰지 않았고,  
앱 전용 폰트가 있었다. 그래서 info.plist에 폰트를 추가하는 식으로 작업했는데,  
FontBlaster를 쓰면 얼마나 편해지길래 라이브러리까지 쓰나 싶어서 도입하기로 결정 ~~!

### 설치하기

```
pod 'FontBlaster'
```

podFile에 추가해준다. 그 후 `pod install`

### 폰트 추가하기

워크스페이스를 열고,

<img src="https://user-images.githubusercontent.com/28949235/130189876-67bdd7ed-173f-4187-9641-2116c9d9d007.png" alt="image" style="zoom:50%;" />

Font 폴더에 원하는 폰트 파일들을 추가해준다.  
그 후 AppDelegate.swift 파일의 **didFinishLaunchingWithOptions()** 함수 내부에 blast() 함수를 호출해줄건데,  
FontBlaster의 blast() 함수가 앱에 폰트를 load 해 주는 역할을 한다.

근데 폰트의 파일명과 실제 폰트의 이름이 다른 경우가 종종 있어서, 먼저 이를 확인해준다.

```swift
import FontBlaster

FontBlaster.blast() { (fonts) in
  print(fonts) // fonts is an array of Strings containing font names
}
```

이렇게 작성해준 후 앱을 빌드 해 보면,

![image](https://user-images.githubusercontent.com/28949235/130191933-0590c27a-d98f-4aca-8f20-4aeeba21daf5.png)

이렇게 앱에 폰트 이름들이 찍혀 나오는 걸 확인할 수 있다.

![image](https://user-images.githubusercontent.com/28949235/130192016-c6741483-aa0b-41fe-a510-a5decc05a221.png)

그럼 completion handler 부분은 지워준다.

이러면 끝 !!! plist에 하나하나 추가하던 시절은 이제 안녕 ~ ~  ~~, ,,!!

