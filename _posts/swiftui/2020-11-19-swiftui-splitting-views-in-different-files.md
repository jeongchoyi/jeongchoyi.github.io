---
layout: post
title: "SwiftUI view별로 각각 다른 파일로 분리하기" 
date: 2020-11-19
category: read 
excerpt: ""

---

# SwiftUI - Splitting Views in different files

> if(kakao)2020을 보고 도전 ( ◠‿◠ ) 

![image](https://user-images.githubusercontent.com/28949235/99618108-13126180-2a64-11eb-9252-9b2a1c3679f0.png)

꼴랑 맨 위에 3줄 짰는데 벌써 길어진 코드수... 속성들 1개당 한 줄씩 차지하다보니 너무 지저분해졌다.  
이걸 그냥 한 파일에 View 변수를 선언해서 쓸 수도 있지만, apple이 권장하는 방법으로 해보기로 했다.

<img src="https://user-images.githubusercontent.com/28949235/99618235-49e87780-2a64-11eb-8944-85b619c3c43a.png" alt="image" width=500 />

요렇게 깔끔한 View를 위해서!



기존의 코드를 먼저 보자. ‼️ 표시를 해둔 부분을 중심으로!

```swift
struct ContentView: View {
    
  	// ‼️ 다른 파일로 분리할 뷰에서 사용하는 변수들
    @State private var progressValue = 25.0
    @State private var progressTotal = 100.0
    
    var body: some View {
        ZStack{
            Color(red: 235/255, green: 235/255, blue: 235/255)
            VStack{
                Rectangle()
                    .fill(Color.white)
                    .frame(width: UIScreen.main.bounds.width, height:211)
                Spacer()
            }
            VStack{ // ‼️ 다른 파일로 분리하고싶은 VStack
                Text("D-78")
                    .font(.system(size: 30))
                    .fontWeight(.semibold)
                    .frame(height:47)
                    .padding(.top, 28)
                Text("다음 회고: 20.11.16 월")
                    .font(.system(size: 12))
                    .fontWeight(.medium)
                    .foregroundColor(.gray)
                ProgressView(value: progressValue, total: progressTotal)
                    .padding([.leading, .trailing], 33)
                    .progressViewStyle(HoneyBeeProgressViewStyle(value: $progressValue, total: $progressTotal)) // ‼️ HoneyBeeProgressViewStyle을 사용했다
                Text("조금만 더 힘내요! 현재 25% 달성했어요.")
                    .font(.system(size: 13))
                Spacer()
            }
            
        }
    }
}

//‼️ HoneyBeeProgressViewStyle 구현부
struct HoneyBeeProgressViewStyle: ProgressViewStyle { 
    @Binding var value: Double
    @Binding var total: Double
    
    func makeBody(configuration: Configuration) -> some View {
        let offset = CGFloat(value) / 100
        return GeometryReader{ geometry in
            VStack(spacing:0){
                HStack{
                    Text("🐝")
                        .font(.system(size: 21))
                        .scaleEffect(x: -1, y: 1, anchor: .center)
                        .frame(width: CGFloat(geometry.size.width * offset + 15), height: 30, alignment: .bottomTrailing)
                    Spacer()
                    Text("🍯")
                        .font(.system(size: 23))
                        .frame(width: 24, height: 30, alignment: .bottomTrailing)
                }
                ProgressView(configuration)
                    .accentColor(.yellow)
            }
        }
        .frame(height: 40)
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```



코드를 살펴봤으니 이제 다른 파일로 분리해보자!

### 파일 만들기

`File` - `New` 

![image](https://user-images.githubusercontent.com/28949235/99618585-eb6fc900-2a64-11eb-9e10-f90b6ec82e6c.png)

**SwiftUI View**를 만들어주면 된다. - `HomeProgressView.swift`

만든 뷰 파일에 우리가 빼야 할 부분들을 다 빼준다.  
SwiftUI View 파일을 만들면 처음 프로젝트를 열었을 때 나오는 ContentView.swift 초기 형식이랑 똑같기 때문에,  
View 부분은 body 내부에 넣어주면 되고,  
변수나 그 외 다른것들도 ContentView.swift 에 있던 같은 위치에 넣어주면 된다.

```swift
import SwiftUI
struct HomeProgressView: View{
  	//변수
    @State private var progressValue = 25.0
    @State private var progressTotal = 100.0
    
    var body: some View {
      	//VStack
        VStack{
            Text("D-78")
                .font(.system(size: 30))
                .fontWeight(.semibold)
            Text("다음 회고: 20.11.16 월")
                .font(.system(size: 12))
                .fontWeight(.medium)
                .foregroundColor(.gray)
            ProgressView(value: progressValue, total: progressTotal)
                .padding([.leading, .trailing], 33)
                .progressViewStyle(HoneyBeeProgressViewStyle(value: $progressValue, total: $progressTotal))
            Text("조금만 더 힘내요! 현재 25% 달성했어요.")
                .font(.system(size: 13))
            Spacer()
        }
    }
}

//ProgressViewStyle
struct HoneyBeeProgressViewStyle: ProgressViewStyle {
    @Binding var value: Double
    @Binding var total: Double
    
    func makeBody(configuration: Configuration) -> some View {
        let offset = CGFloat(value) / 100
        return GeometryReader{ geometry in
            VStack(spacing:0){
                HStack{
                    Text("🐝")
                        .font(.system(size: 21))
                        .scaleEffect(x: -1, y: 1, anchor: .center)
                        .frame(width: CGFloat(geometry.size.width * offset + 15), height: 30, alignment: .bottomTrailing)
                    Spacer()
                    Text("🍯")
                        .font(.system(size: 23))
                        .frame(width: 24, height: 30, alignment: .bottomTrailing)
                }
                ProgressView(configuration)
                    .accentColor(.yellow)
            }
        }
        .frame(height: 40)
    }
}

//이건 나중에 지워도 됨
struct HomeProgressView_Previews: PreviewProvider {
    static var previews: some View {
        HomeProgressView()
    }
}

```



### ContentView 파일에서 불러와서 사용하기

이제 뷰를 분리했으니 `ContentView.swift` 파일은 이런 상태가 된다.  
(분리한 부분은 당연히 지워준 상태)

```swift
import SwiftUI

struct ContentView: View {
    
    var body: some View {
        ZStack{
            Color(red: 235/255, green: 235/255, blue: 235/255)
            VStack{
                Rectangle()
                    .fill(Color.white)
                    .frame(width: UIScreen.main.bounds.width, height:211)
                Spacer()
            }
          	//원래 뷰가 있던 부분
          	HomeProgressView //이렇게 하면 되겠지?
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

난 카카오 스샷만 보고.. 원래 뷰가 있던 부분에 `HomeProgressView`만 써주면 될 줄 알았다.  
근데 그럼 ZStack쪽에서 이런 오류가 난다.

![image](https://user-images.githubusercontent.com/28949235/99618997-d7789700-2a65-11eb-8087-50f3f7bca5d2.png)

> Type 'HomeProgressView.Type' cannot conform to 'View'; only struct/enum/class types can conform to protocols



`HomeProgressView` 이렇게만 쓰면, `HomeProgressView` 타입을 사용하는 것,,,  
나는 지금 타입이 아니라 View를 써야하는 거니까,  
object를 초기화해서 View를 사용하면 된다. : `HomeProgressView()` 

```swift
struct ContentView: View {
    var body: some View {
        ZStack{
            Color(red: 235/255, green: 235/255, blue: 235/255)
            VStack{
                Rectangle()
                    .fill(Color.white)
                    .frame(width: UIScreen.main.bounds.width, height:211)
                Spacer()
            }
            HomeProgressView() //이렇게
        }
    }
}
```

엄청엄청 깔끔해진 모습! 위의 Rectangle()도 다 만들고 나면 분리할 예정이다.  

![image](https://user-images.githubusercontent.com/28949235/99619232-57066600-2a66-11eb-893d-30448a6b347f.png)

> 잘 나온다 ( ◠‿◠ )  



### 더 알아보고 싶은 것

* **fileprivate** struct HomeProgressView: View {} 연결하는 법