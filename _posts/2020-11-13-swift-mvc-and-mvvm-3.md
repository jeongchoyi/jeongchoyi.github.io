---
layout: post
title: "SwiftUI MVVM 패턴 코드 이해하기" 
date: 2020-11-12
category: read 
excerpt: ""

---

# SwiftUI MVVM 패턴 코드 이해하기

[이 글](https://iamcho2.github.io/2020/11/12/swift-mvc-and-mvvm-2)과 이어지는 글

## MVC랑 MVVM은 코드에서 어떻게 다를까?

MVC 패턴을 적용해서 어떻게 통신을 진행했는지 되돌아봤다.  
MVC랑 MVVM을 코드로 비교해가면서 차이점을 이해해보자 ~!

### MVC

Model - Soptoon.swift

```swift
import ObjectMapper

struct Soptoon: Mappable {
    
    var idx: Int?
    var title: String?
    var thumbnail: String?
    var isFinished: Int?
    var likes: Int?
    var author: String?
    
    init?(map: Map) {}
  
    mutating func mapping(map: Map) {
        idx <- map["idx"]
        title <- map["title"]
        thumbnail <- map["thumnail"]
        isFinished <- map["isFinished"]
        likes <- map["likes"]
        author <- map["name"]
    }
}
```



ViewController.swift

```swift
// MVC 패턴에서의 ViewController.swift는 대략 이렇게 생겼다
import UIKit

class SoptoonMainVC: UIViewController {
  
  	// @IBOutlet 변수 선언부 (생략)
    
    var soptoonList = [Soptoon]()
    
    override func viewDidLoad() {
        super.viewDidLoad()

        //여러 함수 호출이나 권한 위임 등...(생략)
    }
  
  	override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        
        getSoptoon(flag: 1)
    }
  
  	// 뷰 관련 함수들 (생략)
    
    func getSoptoon(flag: Int) {
        MainService.shared.getSoptoon(flag: flag) {
            [weak self]
            (data) in
            guard let `self` = self else { return }
            
            self.soptoonList = data
            self.soptoonCV.reloadData()
        }
    }
  
  	//@IBAction 함수 선언부 
  	@IBAction func typeBtnAction(_ sender: UIButton) {
        
        if !sender.isSelected {
            sender.isSelected = true
        }
        
        switch sender.currentTitle {
        case "인기":
            getSoptoon(flag: 1)
            newBtn.isSelected = false
            endBtn.isSelected = false
        case "신작":
            getSoptoon(flag: 2)
            popularBtn.isSelected = false
            endBtn.isSelected = false
        case "완결":
            getSoptoon(flag: 3)
            popularBtn.isSelected = false
            newBtn.isSelected = false
        default:
            break
        }
        
        popularBtn.isSelected ? (popularBtn.backgroundColor = UIColor.maize) : (popularBtn.backgroundColor = UIColor.white)
        newBtn.isSelected ? (newBtn.backgroundColor = UIColor.maize) : (newBtn.backgroundColor = UIColor.white)
        endBtn.isSelected ? (endBtn.backgroundColor = UIColor.maize) : (endBtn.backgroundColor = UIColor.white)
    }
  
  	// extension 으로 여러 DataSource, Delegate 프로토콜 구현부 (생략)
```

실제로는 정말 엄청 엄청 길다.



### MVVM

MVVM에서는 위 코드를 **View**와 **ViewModel**로 분리해서 구현한다.  

자자 다시 정리해보자면  
**Model**: 실제 state 컨텐츠를 나타내고 비즈니스 로직을 구현하는 부분  

**ViewModel** : Model로부터 데이터를 가져오는 부분   
View에서 Model로 이벤트를 전달하기 위해 사용되는 input 객체에 의해 반응하는 action들   

**View**: CollectionView, TableView 등에 데이터가 뿌려지는 부분  
UI의 레이아웃, 구조, 모양 등 정의, 각 뷰에는 특정 뷰의 state를 제공하는 ViewModel 존재  
사용자 상호 작용에 대해 ViewModel에게 알려줌



```swift
//Model
struct Book: Identifiable {
    var id: Int
    var title: String
    var author: String
    var price: Double
    var imageName: String
}
```

필요에 따라 여러가지 <모델>.swift 파일이 나올 것~ 책이면 책 세부사항이나 장바구니나 이런 것들

> 참고 : [SwiftUI의 List](https://iamcho2.github.io/2020/11/06/swiftui-list-foreach)에 대해 배웠듯이, SwiftUI의 List는 DataSource나 Delegate가 필요 없다  
> **Identifiable** 프로토콜을 채택하기만 하면 각 row를 구별할 수 있다



그 다음, 앱의 기능을 정의할 use cases들을 그룹지어서 service를 만드는데,  
그걸 고려해서 프로토콜을 작성한다

```swift
protocol BookService {
    // Book list
    func bookList() -> [Book]

    // Book detail
    func bookDetails(bookId: Int) -> BookDetail
    func numberOfCartItems() -> Int
    func addToCart(bookId: Int)

    // Cart
    func cartItems() -> Cart
    func checkout()
}
```



그리고 가짜 데이터 집어넣고 프로토콜 구현하는 swift 파일  
비즈니스 로직 구현부 (이것도 Model 부분임)

```swift
class MockBookService: BookService {

    // MARK: Mock data
    var books: [Book] = [...]

    var booksDetail: [BookDetail] = [...]

    var cart = Cart(items: [], numberOfItems: 0, total: 0)
    
    // MARK: Book details
    func bookDetails(bookId: Int) -> BookDetail {
        let details = booksDetail.first{ $0.bookId == bookId }
        return details!
    }

    func numberOfCartItems() -> Int {
        return cart.numberOfItems
    }
  
 		(생략)
}
```



이제 View Model을 짤건데, [여기](https://levelup.gitconnected.com/building-an-ios-app-using-swiftui-combine-mvvm-part-2-a0a703269907)서는 MvRx 접근법을 따라서 View Model을 구현했다

> global app state를 사용하지 않고, 그 대신 view-specific한 state를 사용한다

```swift
//MVVM - ViewModel
import Combine
import Foundation

protocol ViewModel: ObservableObject where ObjectWillChangePublisher.Output == Void {
    associatedtype State //특정 view의 state를 참조
    associatedtype Input //사용자의 action이 trigger 함수를 통해 이 입력값을 trigger함
  	// associatedtype을 사용해서 이 type들에게 placeholder 이름을 붙여줌
  	// 프로토콜이 채택되기 전에는 실제 type이 지정되지 않음

    var state: State { get }
    func trigger(_ input: Input)
}

```

ViewModel을 가능한 한 flexible하게 만드는 게 중요한데,  
그 말은 즉 View 컴포넌트를 수정하지 않고도 View Model의 state와 action을 바꿀 수 있게 하는 것~!  
그래서 위와같이 **ViewModel 프로토콜**을 작성한다.

Combine이라는 게 등장했는데, 이 프레임워크를 사용해서 state 업데이트 이벤트를 처리한다.  

`ViewModel` 프로토콜은 `ObserableObject` 프로토콜을 따르는 걸 볼 수 있는데,  
[apple docs](https://developer.apple.com/documentation/combine/observableobject)에서 보면 A type of object with a publisher that emits before the object has changed. 라고 한다.  
즉, 우리의 객체가 변하기 전에 변경된 값을 내보내는 publisher를 포함한다는 것.

> 변수에 @Published를 사용하면 변수의 값이 추가되거나 삭제되었다는 것을 View가 알 수 있게 해준다!



그 다음으로는  `AnyViewModel`이라는 타입을 만들것  
이건 `ViewModel` 프로토콜을 따르는 wrapper처럼 작동하는 녀석,,  
associated types 가 generic type `State`랑 `Input`이 됨

```swift
//MVVM - ViewModel 이어서
final class AnyViewModel<State, Input>: ViewModel {

    // MARK: Stored properties
    private let wrappedObjectWillChange: () -> AnyPublisher<Void, Never>
    private let wrappedState: () -> State
    private let wrappedTrigger: (Input) -> Void

    // MARK: Computed properties
    var objectWillChange: AnyPublisher<Void, Never> {
        wrappedObjectWillChange()
    }

    var state: State {
        wrappedState()
    }

    // MARK: Methods
    func trigger(_ input: Input) {
        wrappedTrigger(input)
    }

    subscript<Value>(dynamicMember keyPath: KeyPath<State, Value>) -> Value {
        state[keyPath: keyPath]
    }

    // MARK: Initialization
    init<V: ViewModel>(_ viewModel: V) where V.State == State, V.Input == Input {
        self.wrappedObjectWillChange = { viewModel.objectWillChange.eraseToAnyPublisher() }
        self.wrappedState = { viewModel.state }
        self.wrappedTrigger = viewModel.trigger
    }
}
```

>  stored properties랑 computed property가 있는데,, 이에 관해서는 [여기](https://zeddios.tistory.com/245)를 참고하자. 

stored properties를 먼저 보면, `wrappedObjectWillChange`함수가 `Publisher`에 대한 어떤 특정한 구현을 반환하는 걸 볼 수 있다,, 그 `Publisher` 의 세부사항은 API 바운더리 밖으로는 공개하고 싶지 않은 것 !  
`AnyPublisher<Void, Never>` 에서 output이 Void형이고, Failure는 Never 타입이므로 fail 할 일이 없다.  
마지막으로 각 특정 구현에 사용할 것은 state 변수와 trigger (_ input :) 함수이다



자 이제 common한 (?) View Model을 다 짰는데,  
개념 정리할때도 봤겠지만 View Model과 View는 일대일 매칭이 된다.  
이제는 특정 View와 매칭되는 View Model을 구현해보자!  
위 코드를 바탕으로 각각에 대해 별도의 State와 Input을 정의하는 것



우선 뷰를 짤건데, 아래와 같은 모양의 BookRow.swift 라는 파일에 row view를 만든다.  
(View 부분, 코드는 생략 - [여기](https://levelup.gitconnected.com/building-an-ios-app-using-swiftui-combine-mvvm-part-2-a0a703269907)에 있음)

<img src="https://user-images.githubusercontent.com/28949235/99026038-85d79480-25ad-11eb-802c-c722ea154e3c.png" alt="image" />

몇개의 element들이 있는 view를 짠건데,  
아직까지는 그냥 `let book: Book` 해서(Book은 우리가 짠 Model) 그걸 view property로 쓰면 되기 때문에  
아직까지는 `ViewModel`이 필요가 없는 상태.

이제 이 element들을 collection으로 보여주는 List를 만들어보자 !  
( 당연히 이 element들에 대해서는 위에서 만든 `BookRow`를 사용할 것 )  
collection에 포함된 각 element들에 대한 view를 제공하는 closure도 만들거다.



```swift
// View - BookListView.swift
import SwiftUI

struct BookListState {
    var service: BookService
    var books: [Book]
}
```

view state를 정의하는 것 부터 시작한다.  
`service`는 모든 view state에 존재하는데, 우리가 data layer에 접근해야 하기 때문이다.  
그리고 화면에 나타날 `Book`의 list도 있다!



```swift
@ObservedObject
var viewModel: AnyViewModel<BookListState, Never>
```

그리고 `viewModel` 프로퍼티를 정의할건데,  
이 때 state를 바꾸는 user input이 없기 때문에 input type으로 Never를 사용할 수 있다!



이제 Book List에 매칭되는 View Model을 짜보자  

```swift
//Book List의 ViewModel
import Foundation

class BookListViewModel: ViewModel {

    @Published
    var state: BookListState

    init(service: BookService) {
        let books = MockBookService().bookList()
        self.state = BookListState(service: service, books: books)
    }

    func trigger(_ input: Never) { }
}
```

정적인(static) state만 제공하기 때문에 구현이 되게 간단한 모습,,
위에서 말했던 것 다시 리마인드하면,  
변수에 @Published를 사용하면 변수의 값이 추가되거나 삭제되었다는 것을 View가 알 수 있게 해준다



그리고 최종적으로 나온 BookListView 코드는 아래와 같다

```swift
import SwiftUI

struct BookListState {
    var service: BookService
    var books: [Book]
}

struct BookListView: View {

    @ObservedObject
    var viewModel: AnyViewModel<BookListState, Never>
  	// state를 바꾸는 user input이 없기 때문에 input type으로 Never를 사용

    var body: some View {
        NavigationView {
            List(viewModel.state.books) { book in
                NavigationLink(destination: NavigationLazyView(BookDetailView(service: self.viewModel.state.service, bookId: book.id))) {
                    BookRow(book: book) //BookRow.swift
                }
            }
            .navigationBarTitle("Book list")
        }
    }
}
```

위에서 Model 만들 때 `Book` data type이 `Identifiable` 프로토콜을 채택했던 걸 기억하자 ~!  
그 덕분에 List에 값들을 띄울 수 있다.

클로저에서 BookRow를 반환해서 동적으로 만들어진 List를 반환한다.  
viewModel.state.books 배열의 각 element에 대해 하나의 BookRow를 생성!  

NavigationLazyView는 그냥 SwiftUI가 destination 뷰를 바로 로드하지 못하게 사용한 것..  
[SwiftUI NavigationLink가 클릭 없이 destination 뷰를 로드할 때](https://stackoverflow.com/questions/57594159/swiftui-navigationlink-loads-destination-view-immediately-without-clicking/61234030#61234030)  여기 참고



이해가 될랑 말랑 



