---
layout: post
title: "SwiftUI : Stack, Text, Image" 
date: 2020-11-4
category: read 
excerpt: ""
---

# SwiftUI : **`Stack`, `Text`, `Image`**

## Stack

![image](https://user-images.githubusercontent.com/28949235/98191824-5a292e80-1f5d-11eb-9998-5031a0542891.png)

![image](https://user-images.githubusercontent.com/28949235/98191848-6ad9a480-1f5d-11eb-87ef-07bea6ab4bb7.png)

![image](https://user-images.githubusercontent.com/28949235/98191864-72994900-1f5d-11eb-81af-b0009363e0db.png)

* 순서대로 VStack{}, HStack{}, ZStack{}

  ```swift
  VStack{
    Text{"Hello, World!"} //ZStack일 땐 여기가 가장 밑에 깔림
    Text{"Hello, World!"}
    Text{"Hello, World!"}
  }
  ```

* 화면 구성할 때 가장 기본 

* 중첩 사용 가능

#### color 넣기 : `Color`

```swift
Color.init(red: DOUBLE(xxx/255), green: DOUBLE, blue: DOUBLE)
Color.red
```

Stack에 Color 넣으면 전체 뷰의 배경색이 되는데, Stack에만 배경색을 주고싶으면

```swift
Text("world!")
	.background(Color.green) //보통 이렇게 밑줄에 표시 - 설정이 여러 개 올 수 있음
```

safearea도 무시하고 전체 뷰에 배경색을 주고싶다면

```swift
ZStack{
  Color.green
  	.edgesIgnoringSafeArea(.all) //위아래 다 무시
  	.edgesIgnoringSafeArea(.top) //위만 무시
  	.edgesIgnoringSafeArea(.bottom) //아래만 무시
}
```

> vstack으로 하면
>
> ![image](https://user-images.githubusercontent.com/28949235/98192555-94df9680-1f5e-11eb-83ca-9c52f3b59163.png) 이렇게 됨.. .all으로 했는데도배경을 다 못채움 -> Color랑 Text가 VStack인거니까

> 그래서 배경을 깔고 그 위에 뭘 하려고 하면 ZStack 이용

### padding 값 주기

```swift
VStack{}.padding() //기본값 : 네방향 다
VStack{}.padding(.leading, 20) //지정가능 
Text("").padding(.top, 50) //Text에 padding 주기
```



## Text

```swift
Text("hello")
	.font(.largeTitle) // 폰트 크기 - .caption, .title3, .body ...
	.underline()
	.bold()
	.forgroundColor(.red) // 폰트 색상
	.padding()
	.frame(width:100, height:50) //강제 크기 지정, 넓이/높이 둘 중 하나만 지정 가능, 잘리는 부분은 ... 됨
	.background(Color.green)
	.cornerRadius(10) // 얘는 button이나 image에서 많이 쓰는 거, 값이 아무리 커도 radius/2가 최대 적용(원모양)
	.lineLimit(1) // 얘랑 .frame이랑 쓰면 width만 지정해도 두줄로 안 바뀌고 한줄 고정
	.lineLImit(nil) // 줄 개수 지정 X
	.truncationMode(.head) // .middle, .tail - 어디가 짤려서 ...로 표시될지 지정
	.lineSpacing(30) //줄간격, 음수 가능(그렇다고 겹쳐지진 않음)
	.multilineTextAlignment(.center) // 여러 줄의 text align 설정
```

### 순서를 고려해야 한다

```swift
Text("hi")
	.padding()
	.background(Color.green) // padding 영역만큼 배경색 지정됨

	.background(Color.green)
	.padding() // 아래처럼 됨 - 배경 먼저 잡히고 그 다음 padding 추가
```

![image](https://user-images.githubusercontent.com/28949235/98193371-5f3bad00-1f60-11eb-8b36-abac472f7557.png)

### 텍스트 뷰 안에 표시되는 텍스트의 포맷 설정 - formatter

* 날짜 형식

```swift
struct ContentView = View{
	static let taskDateFormat: DateForamtter = {
		let formatter = DateFormatter()
    formatter.dateStyle = .long
    return formatter
	}()
  
  var dueDate = Date()
  var body: some View {
    Text("Task due date: \(dueDate, formatter: Self.taskDateFormat)")
  }
}
```

![image](https://user-images.githubusercontent.com/28949235/98194309-6cf23200-1f62-11eb-8169-190755c3c0b7.png)

## Image

>  파일명 : 1.png (500*334 px) 라고 가정

```swift
Image("1")
	.resizable() // 아이폰 전체 크기에 꽉차게 됨, 이미지 크기 조절을 위해 거의 필수
	.frame(width: 300) // frame 영역 잡힘, resizable()이랑 같이 써줘야 이미지 크기 변경됨
	// frame 에서 특정 요소 지정 안하면 아이폰 전체크기만큼으로 잡힘
 	.padding() // .frame 안 쓰고 .padding 주는 경우도 있음
  .edgesIgnoringSafeArea(.all)
  .aspectRatio(contentMode: .fit) //.fill 도 있음
```

* SF-Symbol 사용하기

  ```swift
  Image(systemName: "symbol-name")
  Image(systemName: "circle") //example
  	.forgroundColor(.blue) //색상지정
  ```

* UIImage를 통해 이미지 불러오기

  ```swift
  gurad let img = UIImage(named: "1") else {
    fatalError("Fail to load image")
  }
  
  return Image(uiImage: img)
  ```

* 그라데이션 효과 주기 (Image에 넣어야하나 Text에 넣어야하나,,,)

  ```swift
  Text("HELLO")
  	.background(LinearGradient(gradient: Gradieng(colors: [.white, .black]), startPoint: .top, endPoint: .bottom))
  ```



* 이미지 여러 개 VStack에 넣기

  ```swift
  VStack{
  	Image("1")
  	.resizable()
    .edgesIgnoringSafeArea(.all)
    .aspectRatio(contentMode: .fit)
    
    Image("1")
  	.resizable() // 아이폰 전체 크기에 꽉차게 됨
    .edgesIgnoringSafeArea(.all)
    .aspectRatio(contentMode: .fit)
  }
  ```

   ![image](https://user-images.githubusercontent.com/28949235/98194802-8f387f80-1f63-11eb-9aeb-7feae4c59574.png) --> 이미지 사이에 기본적으로 간격이 있음 (Text끼리는 없지만 Image끼리는 있음)

  ```swift
  VStack(spacing: 0){ //간격 없애기
    이미지들
  }
  ```

  더 주려면 image에 padding 줘도 되는거고..

### mask

```swift
Image("1")
	.resizable()
  .edgesIgnoringSafeArea(.all)
  .aspectRatio(contentMode: .fit)
  .mask(
  	Circle() //cornerRadius로 해도 되긴 하지만..
  )
  .padding(.bottom, 20)
```

> mask도 padding과의 순서에 주의

```swift
.mask(
 	VStack(spacing:0){ // Stack에 집어넣어도 됨 - 눈사람 모양..
    Circle()
    Circle()
  } 
)
```

### 도형 그리기

```swift
Rectangle()
	.fill(Color.red)
	.frame(width:300, height:200)

Circle()
	.fill(Color.blue)
	.frame(width:300, height:200)
```

### 배경으로 이미지 사용하기

```swift
Text("helllo!")
	.background(
    	Image("1")
    		.resizable()
    		.frame(width:100, height:100, alignment: .center)
  )
```







