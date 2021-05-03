---
layout: post
title: "SwiftUI progress bar custom하기" 
date: 2020-11-19
category: read 
excerpt: ""

---

# SwiftUI progress bar custom하기

> SwiftUI ProgressView 레퍼런스 너무 없어 ....  ( ･ᴗ･̥̥̥ )

### 구현할 뷰

<img src="https://user-images.githubusercontent.com/28949235/99562240-30671180-2a0b-11eb-84bc-c8dfca5130b1.png" alt="image" width=500 />



검색해보면 [ZStack 써서 View 구조체 짜고 난리를 치길래](https://rscodesios.wordpress.com/2019/07/23/creating-a-progressbar-in-swiftui/) 오... 망했는데? 라고 생각했는데,  
iOS14 에서 추가된 **ProgressView**를 사용하면 된다! 야호

### Basic

우선 기본은 이렇다.

```swift
ProgressView(value: 40, total: 100)
ProgressView("", value: 40, total: 100) // title을 넣을수도 있다.
```

<img src="https://user-images.githubusercontent.com/28949235/99562861-f6e2d600-2a0b-11eb-873d-c73f48ccf986.png" alt="image" width=400 />

요것이 기본 디자인!

나같은 경우에는 위에 벌도 따라다녀야되고, 꿀 이모지도 있어야해서 커스텀을 해야하지만,  
SwiftUI에서 기본적으로 제공하는 ProgressViewStyle이 몇개가 있다.

[`LinearProgressViewStyle`](https://developer.apple.com/documentation/swiftui/linearprogressviewstyle) : 위 사진 같은 가로로 긴 모양

[`CircularProgressViewStyle`](https://developer.apple.com/documentation/swiftui/circularprogressviewstyle) : 동그란 모양 - 주로 로딩 때 돌아가는 원 모양

```swift
.progressViewStyle(LinearProgressViewStyle(tint: .red))
```

이런 식으로 초기에 accentColor값을 지정해 줄 수도 있다.




### ProgressViewStyle

하지만 기본 스타일로는 내가 구현해야 하는 뷰는 택도 없다ㅎㅎ  
ProgressViewStyle 프로토콜을 따르는 새로운 뷰 스타일을 정의해보자

```swift
struct honeyBeeProgressViewStyle: ProgressViewStyle {
    func makeBody(configuration: Configuration) -> some View {
            ProgressView(configuration)
                .accentColor(.yellow)
        }
    }
}
```

이런식으로 ViewStyle을 짜주면 되는데, 그전에 저 뷰를 어떻게 구성할지 생각을 해봤다.

![](https://user-images.githubusercontent.com/28949235/99566527-5f33b680-2a10-11eb-9440-26270a1b1de8.png)

Stack은 이렇게 구성하고,  
벌 indicator는 progressView의 value값이랑 바인딩을 해서 위치값을 주든,  
왼쪽 spacing을 늘리든 해서 조절하면 될 것 같았다.

우선 Stack구조를 짜보자

```swift
struct honeyBeeProgressViewStyle: ProgressViewStyle {
    func makeBody(configuration: Configuration) -> some View {
        VStack(spacing:0){
            HStack{
                Text("🐝")
                Spacer()
                Text("🍯")
            }
            ProgressView(configuration)
        }
    }
}
```

![image](https://user-images.githubusercontent.com/28949235/99566873-c8b3c500-2a10-11eb-875e-d118666fe410.png)

이런 뷰가 나온다. 이제 색상 등 detail한 부분들을 잡아줘야지! 하던 찰나,,,  
벌이 꿀을 안 보고 반대방향으로 날고있었다... 근데 이걸 이미지로 받긴 싫고...  
어떻게하지 고민하다가 scaleEffect로 텍스트를 좌우반전 시켜줬다.

```swift
Text("🐝")
   .scaleEffect(x: -1, y: 1, anchor: .center) //텍스트 좌우반전
```

```swift
ProgressView(configuration)
   .accentColor(.yellow) //progres view tint color
```

progress view의 색상을 정해줄 때, `.foregroundColor` 안먹는다. `.accentColor`로 해야한다 !

![image](https://user-images.githubusercontent.com/28949235/99567367-6ad3ad00-2a11-11eb-81d9-81fb18020473.png)

### 벌 움직이기

```swift
 HStack{
    Text("🐝")
        .font(.system(size: 21))
        .scaleEffect(x: -1, y: 1, anchor: .center)
   			.padding(.leading, CGFloat(value))
```

> 처음에는 이렇게 leading에 padding을 주려고 했었다.
>
> ![image](https://user-images.githubusercontent.com/28949235/99569874-87251900-2a14-11eb-838a-89014c17226f.png)
>
> 근데 이렇게 벌이 일정 padding값 이상에서는 잘리다가, 값이 더 커지면 그냥 가려져 버린다.

```swift
HStack{
    Text("🐝")
        .font(.system(size: 21))
        .scaleEffect(x: -1, y: 1, anchor: .center)
        .frame(width: CGFloat(value), height: 30, alignment: .bottomTrailing)
```

> 그래서 이렇게 frame으로 조절했다. Text를 frame의 오른쪽아래에 고정시키고,  
> frame의 width값을 조절하는걸로. 

value는 바인딩 (property wrapper @State, @Binding 사용)을 해줘야 한다.



### 벌 - progress bar 연동(?)하기

변수에 바인딩 처리를 해서 벌이 변수 `value`에 따라 움직이게는 해 놨는데,  
사실은 progress bar의 value값에 따라 움직여야 한다.  
progress bar의 value/width 값 만큼 `value`를 지정해주면 되겠다고 생각했다.

근데... progress bar의 width값을 어떻게 받아오지?

### GeometryReader

ZStack이랑 View 써서 progress bar 구현하는 포스트에서 많이 본 애... 내가 쓰게 될 줄이야  
[**GeometryReaders**](https://developer.apple.com/documentation/swiftui/geometryreader)는 해당 뷰가 포함되어있는 부모 뷰를 기준으로 view의 frame을 조절할 수 있게 해 준다.

예를 들어 화면의 딱 절반만큼 뷰를 채우고 싶다거나... 할 때 쓰인다.

```swift
GeometryReader(content: { geometry in
    Text("Content")
})
```

요것의 핵심은 바로 `geometry.size.width` , `geometry.size.height` !!  
바로 짜보자

```swift
struct honeyBeeProgressViewStyle: ProgressViewStyle {
    @Binding var value: Double
    @Binding var total: Double
    
    func makeBody(configuration: Configuration) -> some View {
        return GeometryReader{ geometry in
            VStack(spacing:0){
                HStack{
                    Text("🐝")
                        .font(.system(size: 21))
                        .scaleEffect(x: -1, y: 1, anchor: .center)
                        .frame(width: CGFloat(geometry.size.width / 100 * CGFloat(value) + 15), height: 30, alignment: .bottomTrailing)
                    Spacer()
                    Text("🍯")
                        .font(.system(size: 23))
                        .frame(width: 24, height: 30, alignment: .bottomTrailing)
                }
                ProgressView(configuration)
                    .accentColor(.yellow)
            }
        }
    }
}
```

GeometryReader 안에서는 변수를 선언할 수 없다.  
내가 원하는 꿀벌의 위치는 `geometry.size.width / 100 * CGFloat(value) + 15` 이만큼인데,  
저렇게 이 식을 다 때려박자니 너무 지저분해보여서 변수로 빼고 싶었다.  
`geometry.size.width`를 변수 초기화할 때 사용하려면 GeometryReader 내에서 변수를 만들어야하는데,  
그게 불가능하고, 굳이 쓰려면 [geometryProxy](https://stackoverflow.com/questions/57065925/how-to-define-variables-inside-a-geometryreader-in-swiftui)를 쓰면 된다.

근데 나같은 경우엔 `ProgressView(configuration)` 이 있어서 굉장히 복잡해진 상황...  
그래서 `geometry.size.width`를 제외한 수식들을 `makeBody()`의 local 변수로 만들었다.

```swift
func makeBody(configuration: Configuration) -> some View {
        let offset = CGFloat(value) / 100
        return GeometryReader{ geometry in
            VStack(spacing:0){
                HStack{
                    Text("🐝")
                        .font(.system(size: 21))
                        .scaleEffect(x: -1, y: 1, anchor: .center)
                        .frame(width: CGFloat(geometry.size.width * offset + 15), height: 30, alignment: .bottomTrailing)
```

![image](https://user-images.githubusercontent.com/28949235/99613219-82835380-2a5a-11eb-9c71-3d7ff4c0e969.png)

굿 ! ( ◠‿◠ ) 

