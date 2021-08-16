---
layout: post
title: "서버 통신 함수 리팩토링하기 (GenericResponse)" 
date: 2021-08-16
category: read 
excerpt: ""


---

# 서버 통신 함수 리팩토링하기 (GenericResponse)

리팩토링 하는 이유 전에 원래 어떤 상태였는지 적어보자면

### 원래는...

```swift
import Foundation
import Moya

public class LoginAPI {
    
    static let shared = LoginAPI()
    var loginProvider = MoyaProvider<LoginService>()
    
    enum ResponseData {
        case jwt
    }
    
    public init() { }
    
    func postSignIn(completion: @escaping (NetworkResult<Any>) -> Void, email: String, password: String) {
        loginProvider.request(.postSignIn(email: email, password: password)) { (result) in
            
            switch result {
            case.success(let response):
                
                let statusCode = response.statusCode
                let data = response.data
                
                let networkResult = self.judgeStatus(by: statusCode, data, responseData: .jwt)
                completion(networkResult)
                
            case .failure(let err):
                print(err)
            }
            
        }
    }
    
    private func judgeStatus(by statusCode: Int, _ data: Data, responseData: ResponseData) -> NetworkResult<Any> {
            switch statusCode {
            case 200:
                return isValidData(data: data, responseData: responseData)
            case 400..<500:
                return .requestErr(data)
            case 500:
                return .serverErr
            default:
                return .networkFail
            }
        }
    
    private func isValidData(data: Data, responseData: ResponseData) -> NetworkResult<Any> {
        let decoder = JSONDecoder()
        
        guard let decodedData = try? decoder.decode(JwtResponseData.self, from: data)
        else {
            return .pathErr
        }
        return .success(decodedData.data)
    }
}

```

request를 보내는 함수, status code에 따라 분기처리 하는 함수, data를 decode하는 함수  
이렇게 3개의 함수를 만들어 둔 후,

request를 보내고`getAllChallenges()` 그 result가 success라면 (도로 가든 모로 가든 통신이 되기는 했다면)  
통신 후 받은 result 내 status code를 판단한다. (`judgeStatus()`)  
status code가 200이라면, data를 decode한다. (`isValidData()` )

이 때, data를 decode하는 함수인 `isValidData()`를 보면

```swift
private func isValidData(data: Data, responseData: ResponseData) -> NetworkResult<Any> {
    let decoder = JSONDecoder()
        
    guard let decodedData = try? decoder.decode(JwtResponseData.self, from: data)
    else {
    		return .pathErr
    }
    return .success(decodedData.data)
}
```

decode할 때 `JwtResponseData.self` 에 바로 매핑하는 것을 볼 수 있다

### 리팩토링 하려는 이유

위의 API를 호출했을 때 서버에서 오는 값은 다음과 같다.

```json
// 성공 시
{
    "status": 200,
    "data": {
        "jwt": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjp7ImlkIjoiZmNtIHRva2VuIn0sImlhdCI6MTYyOTA5OTQ2OH0.ttxPT7fODQGYqjmnRsEghGTyFUrWOqK8_7Y8OMcalhY"
    }
}
// 실패 시 (400)
{
    "status": 400,
    "message": "비밀번호가 일치하지 않습니다."
}
```

로그인 API말고 다른 API에서는 어떨까?

```swift
// 다른 API에서의 성공 시 response
{
 "status": 200,
 "data": {
   "course": {
     "id": 1,
     "situation": 1, // 현재 코스 진행 상태
     "title": "뽀득뽀득 세균퇴치",
     // 생략
   }
 }
}
// 다른 API에서의 실패 시 response
{
    "status": 403,
    "message": "만료된 토큰입니다. 우리 아기 고앵이 토큰 하나 더 받아와 쪽-"
}
```



종합하자면 성공 시에는

```json
{
    "status": 200,
    "data": {}
}
```

실패 시에는

```json
{
    "status": 403,
    "message": ""
}
```

아래의 형식으로 오는 걸 확인할 수 있다.

이전처럼

```swift
private func isValidData(data: Data, responseData: ResponseData) -> NetworkResult<Any> {
    let decoder = JSONDecoder()
        
    guard let decodedData = try? decoder.decode(JwtResponseData.self, from: data)
    else {
    		return .pathErr
    }
    return .success(decodedData.data)
}
```

이런 식으로 Data model에 바로 매핑해버리면, status code가 400일 때 message를 받을 수 없다.   
서버에서 주는 error message를 확인할 수 있으면 클라 단에서도 에러 처리 테스팅이 더 쉽기 때문에,  
앱잼 기간 내에서는 시간이 없어서 하지 못했던 에러처리 등을 하려고 refactoring을 진행했다.

### 리팩토링 하기

우선 GenericResponse 구조체를 만들어준다.  
이는 위에서 한 것 처럼 status code에 따른 모든 서버 response를 보고 만들어야 한다 
(모든 앱에서 나랑 똑같은 GenericResponse를 쓸 수 있는게 아니라는 말 !!)

```swift
struct GenericResponse<T: Codable>: Codable {
    var message: String
    var data: T?
    
    enum CodingKeys: String, CodingKey {
        case message
        case data
    }
    
    init(from decoder: Decoder) throws {
        let values = try decoder.container(keyedBy: CodingKeys.self)
        message = (try? values.decode(String.self, forKey: .message)) ?? ""
        data = (try? values.decode(T.self, forKey: .data)) ?? nil
    }
}
```

message, data 변수를 만들어주고, 둘 다 옵셔널 처리를 해 준다.  
성공 시에는 data만 있고, 실패 시에는 message만 있기 때문이다.

그리고 data를 decoding 해 주는 부분에서 data model 말고 GenericResponse를 써 주면 된다.

```swift
private func isValidData(data: Data, responseData: ResponseData) -> NetworkResult<Any> {
    let decoder = JSONDecoder()
        
    guard let decodedData = try? decoder.decode(GenericResponse<JwtResponseData>.self, from: data)
    else {
    		return .pathErr
    }
    return .success(decodedData.data)
}
```

근데, 이 함수를 호출하는 곳을 보면

```swift
private func judgeStatus(by statusCode: Int, _ data: Data, responseData: ResponseData) -> NetworkResult<Any> {
    switch statusCode {
    case 200:
    		return isValidData(data: data, responseData: responseData)
    case 400..<500:
				return .requestErr(data)
    case 500:
				return .serverErr
    default:
    		return .networkFail
    }
}
```

status code가 200일 때만 data를 decoding하는 것을 볼 수 있다.  

```swift
case 400..<500:
				return .requestErr(data)
```

status code가 400~500일 땐 data를 담아서 보내주고 있는데,  
이게 decoding 되지 않은 깡통 데이터라 내부의 message 등을 볼 수 가 없다.

isValidData 함수를 없애고 judgeStatus 함수 내부에 decode 과정을 작성했다.  
(함수를 없애지 않고 isValidData를 먼저 사용하는 방식으로 하려다가.. 반환값이 애매해져서 관뒀다.)

```swift
private func judgeStatus(by statusCode: Int, _ data: Data) -> NetworkResult<Any> {
    let decoder = JSONDecoder()
    guard let decodedData = try? decoder.decode(GenericResponse<JwtData>.self, from: data)
    else {
    		return .pathErr
    }
        
    switch statusCode {
    case 200:
            return .success(decodedData.data)
    case 400..<500:
            return .requestErr(decodedData.message)
    case 500:
            return .serverErr
    default:
            return .networkFail
    }
}
```

이렇게 리팩토링 해 주면 함수의 depth(?)도 적어지기도 하는 장점(이 맞는지 잘 모르겠지만.. )도 생긴다.  
JwtResponseData -> JwtData로도 바꿔줬다.

```swift
// MARK: - jwtResponseModel
struct JwtResponseData: Codable {
    let status: Int
    let data: JwtData
}

// MARK: - jwtData
struct JwtData: Codable {
    let jwt: String
}
```

이제 저 JwtResponseData는 지워도 된단 말씀 !!

끝!





### 실패담..

```swift
    private func judgeStatus(by statusCode: Int, _ data: Data, responseData: ResponseData) -> NetworkResult<Any> {
        
        let decodedData = isValidData(data: data, responseData: responseData)
        if decodedData as! String == "decode fail" {
            return .pathErr
        }
        
        switch statusCode {
        case 200:
            return .success((decodedData as! GenericResponse<CourseData>).data)
        case 400..<500:
            return .requestErr((decodedData as! GenericResponse<CourseData>).message)
        case 500:
            return .serverErr
        default:
            return .networkFail
        }
    }
    
    private func isValidData(data: Data, responseData: ResponseData) -> Any {
        let decoder = JSONDecoder()
        
        switch responseData {
        case .course:
            
            guard let decodedData = try? decoder.decode(GenericResponse<CourseData>.self, from: data) else {
                return "decode fail"
            }
            return decodedData
            
        case .courses:
            
            guard let decodedData = try? decoder.decode(GenericResponse<CoursesData>.self, from: data) else {
                return "decode fail"
            }
            return decodedData
            
        case .medal:
            
            guard let decodedData = try? decoder.decode(GenericResponse<MedalData>.self, from: data) else {
                return "decode fail"
            }
            return decodedData
            
        }
    }
```

이렇게 어찌저찌 해보려다가 `(decodedData as! GenericResponse<CourseData>).data)` 
여기서 data model값을 다시 지정해줘야 된다는 문제점이 생겨서... 
이럼 뭐 .. 분기처리 하는 이유가 없기 때문에 ㅠㅜ ㅠ 그냥 코드를 중복으로 작성했다
