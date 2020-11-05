---
layout: post
title: "SwiftUI : Form, NavigationView, @State" 
date: 2020-11-3
category: read 
excerpt: ""
---

# SwiftUI : **`Form`, `NavigationView`, `@State`**

### 프로젝트 생성하기

![image](https://user-images.githubusercontent.com/28949235/97952051-7ef79780-1ddf-11eb-98d4-93816cdef551.png)

프로젝트 생성할 때, 다른 건 다 똑같고 Interface만 SwiftUI로 바꿔주면 된다

![image](https://user-images.githubusercontent.com/28949235/97952240-0513de00-1de0-11eb-814a-a11db7a3c49f.png)

ContentView.swift 가 초기 유저 인터페이스나 UI를 담당하는 파일.

이 프로젝트에서는 여기서만 작업한다

ContentView.swift의 파일을 살펴보면

```swift
import SwiftUI // import문

struct ContentView: View { //View protocol을 따르는 ContentView struct
    var body: some View { // some View.. View가 body라는 property에 return됨
        Text("Hello, world!" //Text()
            .padding()
    }
}

struct ContentView_Previews: PreviewProvider { //앱에 적용되는 건 아니고,canvas로 preview 화면 보기 위해 존재
    static var previews: some View {
        ContentView()
    }
}
```

* View: SwiftUI의 기본 프로토콜, 화면에 뭘 띄우고 싶으면 무조건 사용해야 함
* some View : View Protocol을 따르는 something을 return하겠다는 뜻
  * `some` 키워드 - 어쩔 땐 이거, 어쩔 땐 저거 리턴 못 함. 항상 같은 것만 리턴할 수 있다는 제한이 있음

* Text("") - 화면에 나타나는 정적 텍스트인데, 필요 시 여러 줄도 자동으로 감싸줌

* canvas 단축키 - `option+command+p`

### Form

> 많은 앱들이 사용자에게 여러 종류의 입력값을 요구한다
>
> 환경설정, 차를 어디서 픽업할지, 무슨 음식 메뉴를 고를건지 ... 이럴 때 쓰라고 있는 전용 view type - Form !

* 텍스트, 이미지같은 정적 컨트롤들로 구성된 scrolling lists
* 텍스트 필드, 토글 스위치, 버튼 같은 user interactive한 컨트롤들도 포함 가능!

```swift
var body: some View {
        Form { 				// 이렇게 Form{} 사이에 요소들을 집어넣어주면 된다.
            Text("Hello, world!")
        }
    }
```

`Text("Hello, world!")` 를 여러 번 쓰면 그만큼 row의 개수도 늘어난다.

![image](https://user-images.githubusercontent.com/28949235/97953367-96388400-1de3-11eb-89b4-ccdc89f20868.png)

> 캔버스 진짜 쩐다 ! 노트북 우주선 소리도 쩐다 ! 



![image-20201103145419284](https://user-images.githubusercontent.com/28949235/97962291-2c76a500-1df8-11eb-9329-7460b6f0e7fb.png)

> SwiftUI error - Extra argument in call

근데 만약, `Text("Hello, world!")`를 열 번 이상 쓰는것과 같이 10개 이상의 요소가 생기면

SwiftUI가 **" 야야 그만 ~~!! 10개까진 되는데 11개 부터 그 이상은 안 돼~!! "** 라고 한다.

이건 Form의 요소 뿐만 아니라 SwiftUI 전반적으로 모두 적용되는 rule... **Parent view 안에는 최대 10개의 Child View**...



그럼 10개 이상 넣고 싶으면 어떡하느냐.. Group으로 묶으면 된다. 이것도 매우 쉽다. 

말그대로 Group{}으로 묶어주면 됨  ◠‿◠   

![image](https://user-images.githubusercontent.com/28949235/97953850-157a8780-1de5-11eb-8aad-9dac8122071b.png)

각 Group별로 그 안에 요소가 10개만 안 넘으면 된다. Group에 넣는다고 해서 보이는게 달라지는 건 없다.

그런데 이렇게 나눠놓고 보니.. Swift UITableView에서 했던 것 처럼 Group간에 분리가 됐으면 또 좋겠는거지.. section 처럼..

section...  맞다 ㅋ Group 대신 Section에 넣어주면 된다.

![image](https://user-images.githubusercontent.com/28949235/97953909-507cbb00-1de5-11eb-946a-71e921df4ef9.png)



### Navigation bar

기본적으로 iOS는 시계 부분 밑이나 home indicator ( _______ 이렇게 생긴 애 ) 밑에까지 어디든지 컨텐츠를 배치할 수 있게 해주는데,

보기에 좋을리가 없다... 그래서 SwiftUI는 safearea 내에 컴포넌트들을 위치시킴

![image](https://user-images.githubusercontent.com/28949235/97955083-a7d05a80-1de8-11eb-93e3-8a92a8131662.png)

허지만.. Form을 스크롤하면 저렇게 system clock 밑에 내용이 위치하게 되는데,

이런 걸 고치는 가장 흔한 방법.,, 바로 Navigation bar !



TextView (`Text{}`)를 section안에 넣고 그 section을 Form에 넣었던 것 처럼,

Navigation bar도 같은 방식으로 추가하면 된다. **NavigationView**에다가!

```swift
var body: some View {
    NavigationView {
        Form {
            Section {
                Text("Hello World")
            }
        }
    }
}
```

이렇게 하면 Canvas에는 위에 빈 공간이 크게 생기는데, 여기가 Navigation bar가 들어갈 부분이다.

Simulator를 돌려보면 아래와 같이 스크롤에 따라 Navi bar가 생긴다

![Untitled](https://user-images.githubusercontent.com/28949235/97956306-a6546180-1deb-11eb-9dc2-bfe9dc7db22b.gif)

> 왜 셀이 녹았을까..



자 이제 Navigation bar에 title을 달아보자! ***modifier***를 사용하면 된다. 

*modifier*는 한 가지 작은 차이가있는 일반 메소드인데, **항상 사용하는 모든 항목의 새 인스턴스를 반환**한다.

```swiftui
.navigationBarTitle(Text("SwiftUI"))
```

를 추가해주는데, 이러면 Swift는 navigation bar title(+우리가 추가한 다른 모든 컨텐츠)를 포함한 새로운 Form을 하나 더 만든다.

![image](https://user-images.githubusercontent.com/28949235/97961655-11576580-1df7-11eb-97a2-6e01ed7ed7f1.png)

> navigation bar title

이렇게 글자가 큰 게 기본이고, 스크롤을 하면 우리가 아는 Navigation bar의 title이 된다.

그럼 처음부터 navigation bar처럼 보이게는 어떻게 하느냐,, - displayMode

```swift
.navigationBarTitle("SwiftUI", displayMode: .inline)
```

![image](https://user-images.githubusercontent.com/28949235/97961798-495ea880-1df7-11eb-8acb-ffa04b4175f6.png)

> navigation bar title - displayMode: .inline

그리고 이건 shortcut 버전! (글자 크기는 large임)

```
.navigationBarTitle("SwiftUI")
```

<br>

### 프로그램 state 수정하기

SwiftUI 개발자들 사이에서는 이런 말이 있다고 한다,,

**"view는 view state의 함수이다(views are a function of their state)"**... 처음엔 먼 말인지 모르겠지만 예시를 보자

예를들어 격투 게임을 하고 있다고 치면, 

목숨 몇개를 잃기도 하고, 점수를 좀 얻기도 하고, 템을 얻거나 좋은 무기를 주울수도 있는데, 이런것들은 우리는 **state**라고 할 것!

**게임이 지금 어떤 상태인지 알려주는 설정들의 active collection**

<br>



게임을 끄면 state는 저장될거고,  다시 게임을 켜면 내가 어디에 있었는지를 다시 불러올 수 있다.

근데 우리가 게임을 하는 동안에는 모든 정수들, 문자열들, boolean들 등등이 내가 뭘 하고 있는지를 설명하기 위해 RAM에 저장되는데,

이 모든 걸 state라고 하는 것!

<br>



자자 그래서 SwiftUI의 view는 그들의 state의 함수이다! 라고 하는 건,

UI가 어떻게 생겼는지가 (= 사람들이 보고 상호작용할 수 있는 것들) 프로그램의 state에 의해 결정된다는 뜻이다.

ex) textfield에 이름을 입력하기 전 까지는 '계속' 버튼을 누를 수 없다던가 하는..

<br>



말은 쉬워 보이는데... 사용자가 앱을 오-래 사용하다 보면 여러 가지 버튼들을 누르기도 하고, 로그인하거나 데이터를 새로고침 하기도 하잖아?

state를 저장하기란 그렇게 호락호락한 것이 아니라는 것 -_-;;

정확한 state를 저장하려면 사용자가 수행한 모든 것을 하나하나 되돌려서 따라가야 하는데,,, 이게 말이 되냑오~

그래서 보통의 앱은 상태를 저장하려고 시도조차 하지 않음. 해도 아주 약간만 함

ex) 포토샵의 undo state stack ...



<br>

SwiftUI에 적용시켜봅시다! : title string이 있는 버튼이 있고 버튼을 누르면 실행되는 action closure

```swift
struct ContentView: View {
    var tapCount = 0
    
    var body: some View {
        Button("Tap Count \(tapCount)"){
            self.tapCount += 1
        }
    }
}
```

빌드해봅시다 ! -> 안 됨.

> SwiftUI Error: Left side of mutating operator isn't mutable: 'self' is immutable

왜냐? ContentView는 struct다. 그 말은.. 얘는 상수로 생성됐을거라는 것 immutable(불변)하다

프로퍼티의 값을 바꾸는 struct 함수를 만드려면 `mutating` 키워드를 써줘야 하는데... Swift는 그걸 금지하고 있음

(computed properties (다른 속성에 의해 해당 속성이 정해지는 프로퍼티)를 mutating하게 바꾸는 것을 금지

= `mutating var body: some View {} ` 이런 식으로 못 쓴다..) 걍 안 됨! 허락을 안해줘요!



<br>

그럼 프로그램 실행 동작중에 값을 바꾸고 싶은데 뷰들이 struct라 못바꾸네... 망한거?

ㄴㄴ.. 그래서 Swift가 주는 특별 솔루션 ~! `preperty wrapper`!!

프로퍼티 이전에 위치시키는 특별 속성임

버튼을 몇 번 클릭했는지 같은 간단한 프로그램 state를 저장하기 위해서는, `@State` 라는 SwiftUI의 property wrapper를 쓰면 됨

```swift
struct ContentView: View {
    @State var tapCount = 0
    
    var body: some View {
        Button("Tap Count \(tapCount)"){
            self.tapCount += 1
        }
    }
}
```

<br>

근데 여기서 질문.. 걍 클래스 쓰면 안되나? 이거 약간 편법같은디..

ㄴㄴ.. 걱정마삼 ! SwiftUI를 배우다보면 알게될텐데.. SwiftUI는 내가 만든 struct들을 자주 없애고 다시만들고 할거라서

그걸 작고 간단한 struct로 유지하는 건 성능에 아주 중요하다~

<br>

> p.s.
>
> 프로그램의 state를 저장하는 방법은 여러가지가 있는데, 그중에서 오늘은 @State를 배운 것
>
> @State는 한 뷰에 저장된 간단한 프로퍼티들을 위해 특별히 만들어진 애
>
> 그래서 Apple은 접근제한자 private 를 추가하는 걸 추천함. `@State private var tapCount = 0` 처럼!

