---
layout: post
title: "Swift Playground SwiftUI tutorial" 
date: 2021-04-08
category: read 
excerpt: ""

---

# Swift Playground SwiftUI tutorial

> [이 영상](https://www.youtube.com/watch?v=me66R7uMyAc) 을 보면서 공부한 것 기록하기

[유튜브 플레이리스트](https://www.youtube.com/watch?v=k_1tqM6LmV0&list=PLZw7eGQJuMjlEcsJ0CgYtxQHgrw1t8aSc) 에서 작년 수상작들의 유튜브 영상을 모아서 볼 수 있다.  
기획 생각할 때 좋을 듯!

[WWDC SwiftUI tutorial](https://developer.apple.com/videos/play/wwdc2020/10643/) 도 있다.

### Shape 템플릿 코드 이해하기(Swift)

![image](https://user-images.githubusercontent.com/28949235/114051368-ff0eb580-98c7-11eb-8044-2f5cd81793bf.png)

한줄한줄 따라가면 쉽다.  

```swift
// Creates a label.
let label = Label(text: "Hello, World!", color: #colorLiteral(red: 1.0008043050765991, green: 0.4036714434623718, blue: 0.6243900656700134, alpha: 1.0), font: .Futura, size: 35)
scene.place(label, at: Point(x: 0, y: 350))
```

라벨을 생성하고, scene.place 함수를 통해서 화면에 띄운다.  
다른 도형들도 마찬가지!

```swift
// Creates a custom shape graphic.
let customShape = Graphic.customShape(path: oddShape(), color: #colorLiteral(red: 0.2275036573, green: 0.5323064327, blue: 0.9950136542, alpha: 1.0))
scene.place(customShape, at: Point(x: 0, y: -325))
```

커스텀 도형도 마찬가지다.

![image](https://user-images.githubusercontent.com/28949235/114051774-5ca30200-98c8-11eb-8d71-f5e7f04f218f.png)

이렇게 interactive한 도형들도 만들 수 있는데, setHandler()함수를 사용해서  
회전, 이동 등의 핸들러 함수를 만들 수 있다.

![image](https://user-images.githubusercontent.com/28949235/114051972-8bb97380-98c8-11eb-90cc-4f35f700d63c.png)

이 파일들을 클릭하면 위 페이지에서 사용한 함수들의 original code들이 나온다.  

### Rock, Paper, Scissors 템플릿 코드 이해하기(Swift)

![image](https://user-images.githubusercontent.com/28949235/114054276-b0164f80-98ca-11eb-9b29-93b30ad69386.png)

이것도 되게 좋은 레퍼런스인데, 가위바위보 게임이다.  
Shape보다는 조금 복잡한 코드!

![image](https://user-images.githubusercontent.com/28949235/114054568-f1a6fa80-98ca-11eb-8881-87964edf42af.png)

book page에 나온 대로 이렇게 코드를 입력하면 게임이 실행된다.  
요 레퍼런스는 나중에 자세히 더 살펴보고, 새 프로젝트를 만들어보자!

### SwiftUI 프로젝트 만들기

Blank 프로젝트를 생성한다.

![image](https://user-images.githubusercontent.com/28949235/114054814-2450f300-98cb-11eb-8f3b-1b7e0837947c.png)

바로 나오는 이 화면에서, `SwiftUI`와 `PlaygroundSupport`를 import 해준다.

> ![image](https://user-images.githubusercontent.com/28949235/114054956-46e30c00-98cb-11eb-9a17-8901ae7d2aee.png)
>
> 자동완성이 화면 맨~ 아래에 있어서 안보인다. 불편 ㅎ

또, 실시간으로 실행하기 위해 SwiftUI로 View를 만들고 PlaygroundPage의 setLiveView 함수를 써줘서  
만든 뷰를 페이지의 현재 라이브 뷰로 설정해준다.

![image](https://user-images.githubusercontent.com/28949235/114055304-9c1f1d80-98cb-11eb-8648-ce208f01f0fc.png)

이렇게 적고 run 해 보면  

> View 타입의 ContentView를 만들고, body를 만든 후 텍스트를 띄워준 내용!

![image](https://user-images.githubusercontent.com/28949235/114055431-bbb64600-98cb-11eb-9397-225099e621a3.png)

바로 옆에서 확인할 수 있다.

이제 원을 만들고 그 원을 클릭했을때 색을 바꿔줘보자!

![image](https://user-images.githubusercontent.com/28949235/114056145-52830280-98cc-11eb-9457-a2fbcebba013.png)

```swift
struct ContentView: View {
    @State var show = false
    
    var body: some View {
        Circle()
            .fill(show ? Color.red : Color.blue)
            .onTapGesture {
                show.toggle()
            }
    }
}

PlaygroundPage.current.setLiveView(ContentView())
```

요렇게 show라는 @State 변수를 선언해준 후, show의 true, false값에 따라  
원의 색상을 정해주고, 탭할때마다 show의 값을 바꿔주는 것!



애니메이션도 줘보자.

![image](https://user-images.githubusercontent.com/28949235/114056522-ba394d80-98cc-11eb-95f8-68eaf1101bc2.png)

```swift
.onTapGesture {
                withAnimation(.easeOut) {
                    show.toggle()
                }
```

`withAnimation` 에 넣어줘서 색이 자연스럽게 애니메이션되면서 변하는 모습을 볼 수 있다.

> 시뮬이 쪼금 로딩시간도 있고 바로바로바로바로 반응이 오진 않는다.

## Xcode에서 개발하기

> 그래.. 확실히 엑코에서 하는 게 낫지 ( ◠‿◠ ) 

이 동영상에서는 Xcode에서 개발하는 방법을 아래와 같이 알려준다.

![image](https://user-images.githubusercontent.com/28949235/114057012-2fa51e00-98cd-11eb-9793-116e5925ab7c.png)

우선 새 App을 만든다.

![image](https://user-images.githubusercontent.com/28949235/114057158-4cd9ec80-98cd-11eb-9357-516d7a0eb74c.png)

여기서는 스유 앱을 만들어주고,

![image](https://user-images.githubusercontent.com/28949235/114057299-67ac6100-98cd-11eb-811f-8b3dcaed1107.png)

그럼 playgrounds 보다 훨씬 친숙한 엑코 등장~!  
코드 하이라이팅도 잘 되고요..~

여기서 작업해서 playgrounds에 복붙하는 것 !

> ㅋㅋㅋ 아놔 난 또 여기서 playground book으로 export하는 줄  
> 작업만 엑코에서 하는거였다.. 근데 그것만 해도 훨 나을 듯..~!



<img src="https://user-images.githubusercontent.com/28949235/114058156-2cf6f880-98ce-11eb-9fa4-4eb280511d81.png" alt="image" style="zoom:50%;" />

요런식으로 카드 뷰를 밑에 복붙해와서, ContentView에 ZStack으로 쌓고  
TapGesture handler 함수를 줄 수도 있다.

### Xcode에서 playground file 만들기

![image](https://user-images.githubusercontent.com/28949235/114059198-169d6c80-98cf-11eb-8359-ed8e858061f9.png)

저번에 공부했던 방법으로도 만들 수 있다.

![image](https://user-images.githubusercontent.com/28949235/114059247-261cb580-98cf-11eb-9430-7837ac50ec82.png)

Playgrounds 앱에서 만드는 방법이랑 똑같은 방법인거다 결국!

![image](https://user-images.githubusercontent.com/28949235/114059372-43518400-98cf-11eb-8e16-cdd8553c04f2.png)

여기다 똑같이 playground 코드를 복사해도 똑같이 preview를 볼 수 있고...  
(근데 frame 문제가 발생하기도 한다)

> 내 생각엔 엑코에서 작업하고 playgrounds에 복붙하는 게 제일 편해보인다.  
> 나는 엑코에서 작업하는 게 익숙하니까..!



### 저장하고 import하기

![image](https://user-images.githubusercontent.com/28949235/114059808-b5c26400-98cf-11eb-9e8e-8e810dd9a95d.png)

이렇게 xcode에서 만든 playground 파일을 Playgrounds에 import할 수 있다.

![image](https://user-images.githubusercontent.com/28949235/114059910-cc68bb00-98cf-11eb-910d-5c7765fe6a45.png)

그냥 드래그 하면 된다 ~!!



내일은 가위바위보 프로젝트를 살펴보자 ~~!!