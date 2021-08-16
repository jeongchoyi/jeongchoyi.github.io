---
layout: post
title: "Networking-4 : Alamofire" 
date: 2021-07-08
category: read 
excerpt: ""

---

# Networking 정리 4 : Alamofire 

## NetworkResult.swift 파일 만들기

> 서버 통신 결과를 처리하기 위한 파일

```swift
import Foundation

enum NetworkResult<T> {
  case success(T)					// 서버 통신 성공했을 때,
  case requestErr(T)			// 요청 에러 발생했을 때,
  case pathErr						// 경로 에러 발생했을 때,
  case serverErr					// 서버의 내부적 에러가 발생했을 때,
  case networkFail				// 네트워크 연결 실패했을 때
}
```

> **\<T>**?  
> 타입 파라미터로, 지금 당장 타입을 정해놓지 않겠다는 뜻  
> 해당 자리에는 Int, String, Bool 등 다양한 타입이 들어갈 수 있는 것

## Model.swift 만들기

Quicktype 등 사용  
CodingKeys, init decode function 등도 여기서 작성

## Service 작성

```swift
struct GetPersonDataService {
  
}
```

내부에 작성

```swift
static let shared = GetPersonDataService()
```

싱글턴 인스턴스 생성 --> 여러 뷰컨에서 shared로 접근하면 같은 인스턴스에 접근할 수 있음

```swift
func getPersonInfo(completion: @escaping (NetworkResult<Any>) -> Void) {
  // completion 클로저를 @escaping 클로저로 정의함
  
}
```

* getPersonInfo의 함수가 종료되든 말든 상관 없이,  
  completion은 탈출 클로저이기 때문에 전달된다면 함수 종료 이후에 외부에서도 사용 가능
* 여기서는 해당 네트워크 작업이 끝날 때  
  completion 클로저에 네트워크 성공 여부 결과(NetworkResult에서 작성한)를 담아서 호출하게 됨.
* 그 담은 네트워크 결과는 이후에 ViewController에서 꺼내서 처리해줄 것

**아래는 함수 내용**

```swift
let URL = "https://어쩌구저쩌구"
let header: HTTPHeaders = [
	"Content-Type": "application/json"
]
```

주소, 헤더 작성

```swift
let dataRequest = AF.request(URL,
                            method: get,
                            encoding: JSONEncoding.default,
                            headers: header)
```

주소, HTTPMethod, 인코딩 방식, 헤더 등  Request를 보내기 위한 정보를 묶어서  
dataRequest에 저장해둔다.  
""나 이렇게 통신 보낼거야!" 라고 적어둔 요청서 개념

```swift
dataRequest.responseData { dataResponse in
  
}
```

위에서 적어둔 요청서를 가지고 진짜 서버에 보내서 Request를 하는 중.  
통신이 완료되면 클로저를 통해 dataResponse라는 이름으로 결과가 도착함.

**아래는 클로저 내용**

```swift
switch dataResponse.result {
  case.success:
  	
  case.failure:
}
```

dataResponse.result (통신 성공했는지 실패했는지 여부)로 switch 분기문을 만들어 준다.  

> dataResponse.result : 통신 성공 여부 (.success, .failure)  
> dataResponse.response?.statusCode: Response의 status code  
> dataResponse.value: Response의 결과 데이터

switch문 내 내용을 작성해보면

```swift
switch dataResponse.result {
  case.success:
  	guard let statusCode = dataResponse.response?.statusCode else {return}
  	guard let value = dataResponse.value else {return}
  	let networkResult = self.judgeStatus(by: statusCode, valule)
  	completion(networkResult)
  
  case.failure: completion(.pathErr)
}
```

**1. 성공일 때**  
우리가 필요한 데이터는 statusCode, response 결과 데이터 두가지.  
그리고 judgeStatus라는 함수에 statusCode랑 response 결과 데이터를 실어서 보낸다.

근데 이 때 성공 실패는 그냥 서버에서 값을 줬냐 안 줬냐 이기 때문에,  
statusCode가 400, 500일 때에도 성공은 성공임. 통신이 되긴 한거니까.  
그래서 judgeStatus로 판단하는 것

**2. 실패일 때** (통신 자체가 실패 했을 때)  
가차없이 `completion(.pathErr)` 통신 실패 값을 담아서 뷰컨으로 날려줌.  
(탈출 클로저니까 클로저가 밖으로 나가는 것이 가능함!)



이제 judgeStatus 함수를 작성하자

```swift
private func judgeStatus(by statusCode: Int, _ data: Data) -> NetworkResult<Any> {
  switch statusCode {
    case 200: return isValidData(data: data)
    case 400: return .pathErr
    case 500: return .serverErr
    default: return .networkFail
  }
}
```

statuscode가   
200일 때 : 성공, 데이터를 가공해서 전달해줘야 하기 때문에 isValidData라는 함수에 데이터를 넘겨 줌.  
					(데이터 가공은 isValidData에서 수행)  
400일 때 : 뭔가 요청이 잘못되었다는 뜻, .pathErr 리턴  
500일 때 : 서버 잘못, .serverErr 리턴  
기타 : .networkFail로 분기처리

여기서 200 말고는 NetworkResult 값을 return하고 있는데, 요 값은 어디로 가냐면  아까 작성한 부분에서

```swift
let networkResult = self.judgeStatus(by: statusCode, valule)
  	completion(networkResult)
```

여기 completion에 실어서 뷰컨으로 보내주는 것 !!  뷰컨에서는 NetworkResult 값을 받아서 분기처리해주면 됨.

이제 데이터를 가공한다고 했던 isValidData 함수를 작성해보자

```swift
private func isValidData(data: Data) -> NetworkResult<Any> {
  let decoder = JSONDecoder()
  
  guard let decodedData = try? decoder.decode(PersonDataModel.self, from: data) else {
    return .pathErr
  }
  // PersonDataModel 형태로 decode를 한 번 해줌. 만약 decode에 실패하면 .pathErr 반환
  
  // 해독에 성공하면 PersonData를 success에 넣어줌.
  return .success(decodedData.data)
}
```

* JSON 데이터를 해독하기 위해 JSONDecoder를 하나 선언 
* data를 우리가 만들어둔 Model 형으로 decode를 시도
* 실패하면 pathErr, 성공하면 decodedData에 값을 담아줌
* decode에 성공하면 success에다가 data 부분을 담아서 completion을 호출함.  
  (그렇게 되면 뷰컨에서 이 data를 빼서 사용 가능)

## 뷰컨에서 서버 통신 진행하기

```swift
GetPersonDataService.shared.getPersonInfo { (response) in 
	switch(response) {
    case .success(let personData):
    	if let data = personData as? Person {
        // 변수에 값 할당, UI 관련 작업 등 수행
        // self.nameLabel.text = data.name
      }
    case .requestErr(let message):
    	print("requestErr", message)
    case .pathErr:
    	print("pathErr")
    case .serverErr:
    	print("serverErr")
    case .networkFail:
    	print("networkFail")
  }
}
```

* 아까 만들어둔 GetPersonDataService 구조체에서 shared 라는 공용 인스턴스에 접근 (싱글턴 패턴)

* 그리고 만들어둔 getPersonInfo를 사용함

  ```swift
  func getPersonInfo(completion: @escaping (NetworkResult<Any>) -> Void) {
    // completion 클로저를 @escaping 클로저로 정의함
    
  }
  ```

  아까 계속 completion 클로저에다가  

  ```swift
  case.failure: completion(.pathErr) //라던가
  
  let networkResult = self.judgeStatus(by: statusCode, value)
  completion(networkResult) // 이런 식으로
  ```

  NetworkResult형 Enum값을 넘겨줬는데, 이제 그 값을 이용해서 분기처리를 해 주면 됨!  
  이미 모든 오류에 대한 처리는 Service에서 해줬으니, VC에서는 결과값에 따라서 분기처리만 하면 됨



* ```swift
  case .success(let personData):
      if let data = personData as? Person {
  ```

  성공했을 경우에는 \<T>형으로 데이터를 받아올 수 있게 해줬는데, (NetworkResult에서)  
  \<T> 형은 Generic하게 아무 타입이 가능하기 때문에  
  우선 클로저에서 넘어오는 데이터를 let personData라고 정의해 둔 후,  
  Person형이니까 거기에 if-let 구문을 통해서 옵셔널 바인딩 해 줌.



## POST를 할 때 GET과의 차이점

### 1. body 보내기

> GET과는 다르게, POST는 body에 데이터를 실어서 요청할 수 있다.  

Service struct 내부에 makeParameter 함수를 만들어준다.

```swift
private func makeParameter(email: String, password: String) -> Parameters {
  return ["email": email,
         "password": password]
}
```

request에 parameters로 makeParameter()를 전달해준다.

```swift
let dataRequest = AF.request(URL,
                            method: .post,
                            parameters: makeParameter(email: email, password: password),
                            encoding: JSONEncoding.default,
                            headers: header)
```

그리고 통신 성공, status code가 200인지 보기 위해 message만 전달하려면

```swift
private judgeStatus(by statusCode: Int, _ data: Data) -> NetworkResult<Any> {
  let decoder = JSONDecoder()
  
  guard let decodeData = try? decoder.decode(LoginDataModel.self, from: data) else {
    return .pathErr
  }
  
  switch statusCode {
    case 200: return .success(decodeData.message)
    case 400: return .requestErr(decodeData.message)
    case 500: return .serverErr
    default: return .networkFail
  }
}
```

성공, 실패시에도 메세지를 담아서 반환



### 추가 : Alert에 cancel, ok Action closure 넘기기

```swift
extension UIViewController {
  func makeRequestAlert(title: String,
                       message: String,
                       okAction: ((UIAlertAction) -> Void)?,
                       cancelAction: ((UIAlertAction) -> Void)? = nil,
                       completion: (() -> Void)? = nil) {
    
    let generator = UIImpactFeedbackGenerator(style: .medium)
    generator.impactOccurred()
    
    let alertViewController = UIAlertController(title: title,
                                               message: message,
                                               preferredStyle: .alert)
    
    let okAction = UIAlertAction(title: "확인", style: .default, handler: okAction)
    alertViewController.addAction(okAction)
    
    let cancelAction = UIAlertAction(title: "취소", style: .cancel, handler: cancelAction)
    alertViewController.addAction(cancelAction)
    
    self.present(alertViewControlelr, animated: true, completion: completion)
  }
  
  func makeAlert(title: String,
                message: String,
                okAction: ((UIAlertAction) -> Void)? = nil,
                completion: (() -> Void)? = nil) {
    
    let generator = UIImpactFeedbackGenerator(style: .medium)
    generator.impactOccurred()
    
    let alertViewController = UIAlertController(title: title,
                                               message: message,
                                               preferredStyle: .alert)
    
     let okAction = UIAlertAction(title: "확인", style: .default, handler: okAction)
    alertViewController.addAction(okAction)
    
    self.present(alertViewControlelr, animated: true, completion: completion)
  }
}
```

이렇게 Extension으로 만들어두면 아무 뷰컨에서나 makeAlert 메서드를 호출할 수 있음.  
VC에서 호출할 땐

```swift
self.makeAlert(title: "알림", message: "로그인 성공")

self.makeRequestAlert(title: "알림",
                     message: "로그인을 하시겠습니까?",
                     okAction: { _ in
                       // ok버튼 눌렀을때 하고싶은 동작
                          self.loginAction() // 통신 처리 메서드 등...
                     })
```



