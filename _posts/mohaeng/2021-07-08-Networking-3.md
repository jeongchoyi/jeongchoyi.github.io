---
layout: post
title: "Networking-3 : Escaping Closure, Singleton Pattern" 
date: 2021-07-08
category: read 
excerpt: ""

---

# Networking 정리 3 :  

# Escaping Closure, Singleton Pattern

### Escaping Closure

> 함수 파라미터로 클로저를 넘길 수 있다. 

```swift
override func viewDidLoad() {
  super.viewDidLoad()
  
  callback {
    print("Closure가 실행되었습니다.")
  }
}

func callback(closure: () -> Void) {
  closure()
}
```



```swift
class ViewController: UIViewController {
  var sampleClosure: () -> Void = {} // 외부에 클로저를 담는 변수를 만들어두고,
  
  override func viewDidLoad() {
    super.viewDidLoad()
    
    callback {
      print("Closure가 실행되었습니다.")
    }
  }
  
  func callback(closure: () -> Void) {
    sampleClosure = closure // 외부 변수에 매개변수 값을 넣으려고 하면 --> 에러 발생
    sampleClosure()
  }
}
```

> 클래스에 sampleClosure 변수를 선언하고, 해당 callBack 함수 내부에서  
> 매개변수로 받은 closure를 대입하려고 하면 에러가 발생함.

Swift 에서는 함수의 매개변수로 전달된 클로저는 기본적으로 해당 함수 내에서만 사용이 가능!  (탈출 불가 상태)  
직접 실행만 가능하고, 외부 변수나 상수에 대입도 안 됨.

원래는 클로저가 매개변수로 넘어가게 되면, 반드시 해당 함수가 끝나서 리턴되기 전에  
해당 클로저는 실행이 됨. --> 해당 함수가 끝나게 되면 파라미터로 넘어간 클로저는 사용 불가!

**해결: @escaping**

매개변수로 넘어온 클로저 앞에 @escaping을 달아주면, 아까 발생하던 오류가 사라짐.  
매개변수로 넘어온 클로저를 밖으로 탈출시키는 게 가능해지는 것!

* 해당 클로저를 외부 변수/상수에 저장 가능
* 해당 함수가 끝나서 리턴 이후에 해당 클로저 실행이 가능
* 함수의 실행 순서를 정할 수 있음
  * 비동기적으로 실행되는 통신 코드에서, 순서를 정할 수 있게 되면서  
    서버에서 데이터를 안전하게 받아온 것을 보장받고 값을 return 할 수 있음!
  * 서버에서 데이터를 받아오는 작업이 끝났을 때 탈출 클로저 호출
* 외부에서도 사용 가능!

### Singleton Pattern

```swift
class UserInfo {
  var id: String?
  var password: String?
  var name: String?
}
```

만약 뷰컨 A,B,C에서 id, password, name을 따로 하나씩 받아야 한다면?  
해당 인스턴스는 최초 생성될 때 전역으로 저장해두고, 이후에는 이 전역 인스턴스에만 접근하게 하면 됨.

그 전역 인스턴스 = 싱글턴 객체

```swift
class UserInfo {
  static let shared = UserInfo() //static으로 인스턴스 선언
  
  var id: String?
  var password: String?
  var name: String?
}
```

```swift
// A VC
let userInfo = UserInfo.shared
userInfo.id = "aaa"
// B VC
let userInfo = UserInfo.shared
userInfo.name = "DDD"
```















