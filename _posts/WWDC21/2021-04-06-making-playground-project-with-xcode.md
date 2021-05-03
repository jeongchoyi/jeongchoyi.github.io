---
layout: post
title: "Swift Playground 프로젝트 xcode에서 만들기" 
date: 2021-04-06
category: read 
excerpt: ""

---

# Swift Playground 프로젝트 xcode에서 만들기

> playground book 만드는 글을 읽다가, 아래와 같은 문장을 읽었다.
>
> 플레이그라운드 북에서 사용하는 Swift 파일들은 UIKit을 import하지만 메소드의 자동 완성 제안이 제대로 동작하지 않기 때문에, 복잡한 콘텐츠를 만들 때는 iOS 앱을 만들어 코드를 충분히 테스트 한 뒤 플레이그라운드 북으로 변환하는 게 좋다.
>
> 그래서 Xcode에서 먼저 만들어보려고 한다!  
> 수상작 프로젝트들 몇개 살펴보니 어떤 건 playgroundbook이 아니고 playground 파일이라서  
> xcode에서 열리던데... 뭔 차이인건지 잘 모르겠다 ◠‿◠ 💦


### Playgrounds 프로젝트 만들기

<img src="https://user-images.githubusercontent.com/28949235/113669443-bd76e280-96ee-11eb-8674-021fff23cd3e.png" alt="image" style="zoom:50%;" />

 Xcode에서 File-New-Playground를 선택한다.

<img src="https://user-images.githubusercontent.com/28949235/113669775-2cecd200-96ef-11eb-9602-6f4f0a52fe6e.png" alt="image" style="zoom:40%;" />

`Single View` 를 선택해준 후, 적절한 이름을 짓고 저장해준다!

<img src="https://user-images.githubusercontent.com/28949235/113669903-5f96ca80-96ef-11eb-9715-254711e88f26.png" alt="image" style="zoom:50%;" />

그럼 이런 파일이 만들어지는데, 파란색 play 버튼을 눌러보면

![image](https://user-images.githubusercontent.com/28949235/113670002-83f2a700-96ef-11eb-8fb9-72549fbadd6b.png)

오른쪽 캔버스에서 미리볼 수 있다. SwiftUI 개발하는 것 처럼 !! 되게 편할 것 같다.

템플릿 코드를 보면, controller 클래스를 생성하고,  
현재 `PlaygroundPage` 의 LiveView에 띄워주는 것을 볼 수 있다.

이제 해야 할 건 페이지들을 추가하고, 코드 작성하고, live view에서 실행하는 것!

[블로그 포스트](https://medium.com/@barbulescualex/using-swift-playgrounds-playground-books-87c2707be2b5)에서 본 주의해야 할 점들이 몇가지 있다.

- Source 폴더에 있는 Classes, structs, enums들은 **꼭 public으로** 선언되어야 한다.  
  그래야 pages가 그것들을 인식할 수 있다.
- Xcode playground는 잘 튕긴다 ^.^  
  특히 조금 작업하고 나서 커맨드+S 눌러서 저장할 때...  
  우리가 할 수 있는 건 애플이 고쳐주길 기다리는 것 뿐...

### Playground books 만들기

[공식 문서](https://developer.apple.com/documentation/swift_playgrounds/creating_and_running_a_playground_book)

[여기](https://developer.apple.com/download/more/?=Swift%20Playgrounds%20Author%20Template) 에 가면 Playground books의 auther templates를 다운받을 수 있다.  
Xcode 12.4 버전은 없어서 난 우선 12.2 버전을 다운받았다.

Mac에서 작업하고, book page를 작성한 후 모든 리소스들을 `.playgroundbook` 파일로 패키징 한 다음  
iPad 등에서 보면 된다고 한다!

<img src="https://user-images.githubusercontent.com/28949235/113672050-26138e80-96f2-11eb-8a3f-e9c276a1ac80.png" alt="image" style="zoom:50%;" />

포함된 파일들은 위와 같다. README.md 부터 읽어보자!

### auther template README 읽기

는 다음 포스트에서...