---
layout: post
title: "Networking-2 : URLSession" 
date: 2021-07-08
category: read 
excerpt: ""

---

# Networking 정리 2 : URLSession

애플이 제공하는 서버 통신 API. `Alamofire` 등의 기반이 되는 API.  
HTTP/HTTPS를 포함한 몇 가지 프로토콜을 지원하고, 인증, 쿠키 관리, 캐시 등도 지원함.

### URLSession

> HTTP 요청의 송신과 수신을 담당하는 핵심 객체.  
> URLSessionConfiguration ( .default, .ephemeral, .background )을 사용해서 생성

### URLSession 순서

1. Session` configuration`을 설정하고, `Session`을 생성한다.
2. 통신할 URL과 Request 객체를 설정한다
3. 사용할 `Task` 를 결정하고, 그에 맞는 `Completion Handler`나 `Delegate` 메소드들을 작성한다
4. 해당 `Task`를 실행한다
5. `Task` 완료 후 `Completion Handler` 가 실행된다

> 간단한 통신일때는 `Completion Handler`를 사용하지만,  
> 앱이 background 상태일때에도 다운로드를 받아야 한다거나... 등등 할 때에는 `Delegate` 패턴을 사용

### 1. Session `configuration` 을 설정하고, `Session` 을 생성한다

URLSession은 configuration이라는 객체를 가지고 있는데,  
업로드 할지 다운로드 할지 등의 행동과 규칙을 정의하는 객체이다.

```swift
let defaultSession = URLSession(configuration: .default)
```

* `.default`: 기본적인 Session으로 디스크 기반 캐싱을 지원
  * delegate 지정 가능 (순차적으로 데이터 처리)
* `.ephemeral` : 어떠한 데이터도 저장하지 않는 형태의 세션. 비공개(private) 세션
* `.background` : 앱이 종료된 이후에도 통신이 이뤄지는 것을 지원하는 세션
  * 앱이 중지되거나 종료되어도 계속함



```swift
let sharedSession = URLSession.shared()
```

이렇게도 많이 생성하는데, 이렇게 singleton으로 생성할 경우  
configuration 객체가 없고, 사용자 정의가 불가능하다.



### 2. 통신할 URL과 Request 객체를 설정한다

```swift
guard let url = URL(string: "\(resourse)") else {
  print("URL is nil")
  return
}

let request = URLRequest(url: url)
```

`URLRequest` 를 통해 서버로 요청을 보낼 때 어떻게 데이터를 캐싱할지,  
어떤 HTTP  메서드를 사용할 지 (GET / POST 등...) 어떤 내용을 전송할 지 등을 설정할 수 있다.

> 기본 요청 메소드는 GET이고,  
> POST, PUT, DELETE로 요청을 보내고 싶으면 HTTPMethod 프로퍼티를 조절해주면 된다.
>
> ```swift
> request.httpMethod = "GET"
> ```
>
> 헤더나 Body 등도 설정할 수 있다.
>
> ```swift
> request.addValue("application/json", forHTTPHeaderField: "Accept")
> ```

### 3. 사용할 `Task` 를 결정하고, 그에 맞는 `Completion Handler`나 `Delegate` 메소드들을 작성한다

> URLSession은 여러 개의 URLSessionTask를 만들 수 있다

* Task 객체 : 일반적으로 Session 객체가 서버로 요청을 보낸 후,  
  응답을 받을 때 URL 기반의 내용들을 받는 역할을 함. 종류는 3가지
  * `URLSessionDataTask ` : Data 객체를 통해 데이터를 주고받는 Task  
    HTTP GET 요청에 의한 작업
  * `URLSessionUploadTask` : Data를 파일의 형태로 전환 후 업로드하는 Task  
    전통적인 HTTP POST 또는 PUT 요청에 의한 작업
  * `URLSessionDownloadTask` : Data를 파일의 형태로 전환 후 다운로드하는 Task  
    백그라운드 다운로드를 지원함. 일시정지도 가능

```swift
// 네트워킹할 url을 매개변수로 받고 네트워킹 요청을 함
// 완료가 되면 수행 작업은 Completion Handler에서 진행
let dataTask = defaultSession.dataTask(with: request) { data, response, error in
                                                       
    // data error
    guard error == nil else {
      print("Error occur: \(String(describing: error))")
      return
    }
                                                       
    if let data = data, let response = response as? HTTPURLResponse, response.statusCode == 200 {
      // 통신에 성공했으면 data에 Data 객체가 전달됨
      do {
        // JSON 타입의 데이터를 디코딩
				let userResponse = try JSONDecoder.decode(UserResponse.self, from: data)
				self.users = userResponse.results
        
        // 원하는 작업 수행
        // UI작업이 있다면 꼭 main 스레드에서 수행
        DispatchQueue.main.async {
          self.tableview.reloadData()
        }
        
      } catch (let err) {
        print("Decoding Error")
        print(err.localizedDescription)
      }
    }
}
```

### 4. 해당 Task를 실행한다

```swift
// resume
dataTask.resume
```

### 5. `Task` 완료 후 `Completion Handler` 가 실행된다

끝 ~~



## Body에 들어가는 파라미터가 두 개 이상일 때

[여기](https://learn-hyeoni.tistory.com/m/42) 참고



## 주의사항

### 비동기

URLSession은 자체적으로 비동기적으로 작동하게 구현되어있음.  
그래서 따로 비동기 처리할 필요가 없다.

대신 completionHandler를 작성할 때, UI관련 작업을 수행한다면  
**반드시 ! main 스레드에서 작업**해줘야 한다.

### configuration을 수정하려면

session을 생성할 때, session이 생성되고 난 이후에 configuration 객체를 바꿔도  
session은 변하지 않는다.

configuration의 복사본으로 session을 셋팅하기 때문이라는데...

아무튼!  configuration 규칙을 수정하고 싶으면,  
새로운 configuration으로 새로운 session을 만들어야 한다.

### class 만들어서 관리하기

근데 보통 URLSession과 같은 네트워킹용 API는 일반적으로 앱 전역에서 사용되기 때문에,  
ViewController에 메소드를 작성하기 보다는 하나의 모듈 (class)를 만들고  
그 안에 static 함수들을 만들어 사용하는 것이 좋다.



### 출처, 나중에 찾아볼 포스트

[슈프림 블로그](https://tngusmiso.tistory.com/50) 

[Request 종류 (GET, POST, DELETE, PUT) 에 종속되지 않은 형태로 구현](https://sueaty.tistory.com/130)

