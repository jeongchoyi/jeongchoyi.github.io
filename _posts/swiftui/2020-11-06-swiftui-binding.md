---
layout: post
title: "SwiftUI : binding, @Binding" 
date: 2020-11-6
category: read 
excerpt: ""

---

# SwiftUI : **`binding`,`@Binding`**

어제 Toggle을 공부할 때 잠깐 나온 **@Binding**에 대해 더 이해해보자!

**@Binding**도 **@State**와 같이 property wrapper 중 하나인데, 우선 binding, 양방향 바인딩에 대해 이해를 먼저 해보자.

```swift
struct ContentView: View {
    @State private var tapCount = 0
    
    var body: some View {
        Button("Tap Count \(tapCount)"){
            self.tapCount += 1
        }
    }
}
```

**@State**는 뷰의 struct들을 자유롭게 바꾸게 해줘서, 프로그램이 변경되면 뷰 프로퍼티들이 그에 맞게 갱신되게 만들 수 있었다.

근데, 여기에 user interface controls이 끼면 일이 좀 복잡해진다,,

예를들어, 사용자들이 타이핑 할 수 있는 editable한 텍스트 박스를 만들고 싶다면?

```swift
struct ContentView: View {
    var body: some View {
        Form {
            TextField("Enter your name")
            Text("Hello World")
        }
    }
}
```

이렇게 하시겠읍니까? 하지만.. 컴파일 오류가 뜬다.

왜냐면 SwiftUI는 text field의 text를 어디다 저장할지를 알고싶어하기 때문!

<br>

저번에 **view는 view의 state들의 함수이다!** 라고 했던 것을 다시 떠올려보면,

textField는 프로그램에 저장되어있는 값을 그냥 보여주기만 할 수 있음.

SwiftUI가 원하는 건 우리 struct에 있는 문자열 프로퍼티가 

textField에도 보여지면서, 유저가 타이핑하는 것들을 저장할 수 있었으면 좋겠는 것.. 동시에 !!!

그럼 아래처럼 코드를 바꿔보자

```swift
struct ContentView: View {
    var name = ""

    var body: some View {
        Form {
            TextField("Enter your name", text: name)
            Text("Hello World")
        }
    }
}
```

> name 프로퍼티 추가

But... 이래도 안 됨.

왜냐면 Swift는 우리가 추가한 name  프로퍼티를 유저가 타이핑한거랑 매치되게끔 **업데이트해야** 하니까..

아하 그러면 **@State** 쓰면 되겠군?

```swift
@State private var name = ""
```

그러나 이것도 Not enough !! 컴파일 안 됨.

<br>

문제가 뭐냐면, Swift는

**"여기에 이 프로퍼티의 값을 보여주세요"**와 **"여기에 이 프로퍼티의 값을 보여주는데, 변경 사항이 있으면 그 프로퍼티에 작성해주세요"**를

별개로 구분짓기 때문 !!!

<br>

예로 들었던 text field에서, Swift는 텍스트가 무엇이 됐든 그게 name 프로퍼티에도 있게 만들어야 한다.

그러면 뷰는 state의 함수다! 라는 약속을 지킬 수 있게 되고,

유저가 볼 수 있는 모든것들이 그냥 우리의 코드에 있는 struct들, 프로퍼티들의 visible한 표현이 되는 것.

<br>

이런 걸 **two-way binding (양방향 바인딩)**이라고 하는데, 

text field가 프로퍼티의 값을 보여주도록 바인딩하지만

text field의 변경 사항도 속성을 업데이트하도록 바인딩 함.

<br>

그래서 나온 최종 코드 !

```swift
struct ContentView: View {
    @State private var name = ""

    var body: some View {
        Form {
            TextField("Enter your name", text: $name)
          	Text("Your name is \(name)") 
        }
    }
}
```

![image](https://user-images.githubusercontent.com/28949235/98327567-2ae6f000-2037-11eb-9838-d6fea8c0179d.png)



프로퍼티 이름 앞에 **$** dollar sign을 보면, 아아 이건 양방향 바인딩이군 ㅋ 하면 됨.

프로퍼티의 값이 읽히는데, 동시에 작성되는 것.



> p.s.
>
> ```swift
> Text("Your name is \(name)") 
> ```
>
> 여기선 왜 $name 안 쓰고 name을 썼나?
>
> 왜냐면 여기서는 양방향 바인딩을 할 필요가 없기 때문.
>
> 우리는 그냥 값을 읽고싶은거지 다시 어디다 저장하고 그러는 걸 원하는 게 아님.

<br>

그럼 이제 어제 배운 Toggle로 돌아가서 코드를 다시 보자!

![image](https://user-images.githubusercontent.com/28949235/98321500-95446400-2028-11eb-8291-d56e72613e6e.png)

```swift
struct ContentView: View {
	@State private var isOn = false

	var body: some View {
  	Toggle("toggle \(isOn.description)", isOn: $isOn)
	}
}
```

바인딩을 해야 될 @State값을 하나 만듦. 바인딩 타입으로 만드려면 사용할 때 변수명 앞에 $를 붙임.

변경되는 값이랑, 변경돼서 보여줘야하는 UI랑 바로 매칭이 되어야 하니까!

<br>

이렇게 toggle을 따로 빼서 **@Binding**을 쓰는 방법도 있음 ~!

```swift
struct ContentView: View {
  @State private var isOn = false
  var body: some View {
    VStack{
      MyToggle(isOn: $isOn)
    	Text("\(isOn.description)")
    }
  }
}

struct MyToggle: View {
  @Binding var isOn: Bool // property wrapper @Binding
  
  var body: some View {
    Toggle("toggle \(isOn.description)", isOn: $isOn)
  }
}
```

* **@Binding**은 = 해서 초기화하면 오류 남. @Binding 은 값을 가질 수 없음. 원본 값이 어딘가 있다는 개념이라..
  * 타입만 지정하고, 본 ContentView에서 MyToggle을 생성할 때 isOn값을 지정해주면 됨.
* 다른 뷰 (MyToggle)에서 값을 변경하지만 실제 변경되는 값은 ContentView의 isOn
