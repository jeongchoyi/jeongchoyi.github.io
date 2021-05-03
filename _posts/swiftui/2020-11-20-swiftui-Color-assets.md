---
layout: post
title: "SwiftUI Color asset 사용하기" 
date: 2020-11-19
category: read 
excerpt: ""

---

# SwiftUI - Color asset 사용하기

<img src="https://user-images.githubusercontent.com/28949235/99898812-bb842800-2ce7-11eb-9fee-c3b357a3ea99.png" alt="image" width=300 />

Color asset으로 색상들을 관리하면, `Color.bgColor` 이렇게 보기 좋게 사용할 수도 있고,  
이렇게 디자이너가 뽑아준 색상을 제플린에서 사용하는 네이밍 그대로  
iOS 프로젝트에서도 사용할 수 있다.



<img src="https://user-images.githubusercontent.com/28949235/99898864-4e24c700-2ce8-11eb-9285-970d21d011c7.png" alt="image" width=600 />

`File - New - FIle...` 이나 `⌘ + N` 을 누르고, 새 파일 만드는 창이 뜨면  
`asset`을 검색해서 `Asset Catalog`를 만든다.

이름은 **`Colors.xcassets`** 로!

<img src="https://user-images.githubusercontent.com/28949235/99898914-9fcd5180-2ce8-11eb-8ead-30320d41fdc1.png" alt="image" width=400 />

\+ 버튼을 누르고 새 Color Set을 만들어주고, 적절한 이름을 지어준다.

![image](https://user-images.githubusercontent.com/28949235/99899152-685fa480-2cea-11eb-8e21-615b5bc28eb4.png)

이렇게 각 색상과, 그 색상이 다크모드일 땐 무슨 색으로 나타날지 지정이 가능하다.  
다크모드...!!!

![image](https://user-images.githubusercontent.com/28949235/99899171-8af1bd80-2cea-11eb-9ddd-46d29347d485.png)

지정해줄 색상을 선택한 후 Attribute Inspector에 들어가서, 아래쪽을 보면

<img src="https://user-images.githubusercontent.com/28949235/99899157-74e3fd00-2cea-11eb-8886-44a821ad106f.png" alt="image" width=400 />

아래쪽에 Color를 지정해줄 수 있다. 나는 HEX값으로 입력하고싶어서  
`Show Color Panel`을 눌러서 Color Panel을 열었다.

<img src="https://user-images.githubusercontent.com/28949235/99898989-43b6fd00-2ce9-11eb-8bd7-44e67fe22517.png" alt="image" width=300 />

이렇게 입력해준 후 엔터를 누르면

![image](https://user-images.githubusercontent.com/28949235/99898995-4c0f3800-2ce9-11eb-86eb-d3f67b768099.png)

이렇게 색이 바뀌어있다 !

> 처음엔 yellow라고 하고 그 후에 mainYellow로 바꾼건데,  
> yellow라고 하면 기존에 있던 Basic Color Asset이랑 겹쳐서 그런가  
> 'yellow'의 사용이 모호하다고 뜬다. 그래서 mainYellow로 바꿈 ㅠ

![image](https://user-images.githubusercontent.com/28949235/99899096-e8393f00-2ce9-11eb-9a26-4d7c7efc0759.png)

이렇게 Extension 파일에다가 Color extension을 만들고, 원하는 색을 static 상수로 선언해준다.



![image](https://user-images.githubusercontent.com/28949235/99899132-32222500-2cea-11eb-8dd5-6fa602936e0d.png)

그렇게 하면 이렇게 바로 사용 가능하다! `.red`나 `.blue` 처럼 ( ◠‿◠ ) 