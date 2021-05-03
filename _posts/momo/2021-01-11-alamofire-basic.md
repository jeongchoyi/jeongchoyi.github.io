---
layout: post
title: "Alamofire 기초" 
date: 2021-01-11
category: read 
excerpt: ""

---

# Alamofire 기초

## APIService

### APIConstants.swift

```swift
struct APIConstants {
    static let baseURL = "http://3.36.79.14:3000"
    // 로그인 url
    static let usersSignInURL = baseURL + "/users/signin"
    // 회원가입 url
    static let usersSignUpURL = baseURL + "/users/signup"
}
```

APIConstants에 통신 URL들을 잘 나눠서 작성!

### GenericResponse.swift

> **데이터를 JSON 데이터 포맷으로 자유롭게 Decoding, Encoding 할 수 있도록 해주는 protocol**

```swift
// 받아온 data를 swift type으로 decoding하기 위한 파일
// 응답에 대한 처리를 해줘서 성공과 실패에 대해 다른 decoding을 해줘야 함.
// 그걸 위해 만든 모델 파일 - GenericResponse.swift
struct GenericResponse<T: Codable>: Codable {
    // Codable: 데이터를 JSON 데이터 포맷으로 자유롭게 Decoding, Encoding 할 수 있도록 해주는 protocol
    var message: String
    var data: T? //요 타입도 임의로 만들어줘야 함
    
    enum CodingKeys: String, CodingKey {
        //json은 key, data값을 가지고있는데
        //json의 key값을 swift 타입으로 디코딩할때 이름이 똑같아야해서
        //CodingKeys를 통해 data변수의 key랑 struct를 이어주는 역할
        case message = "message"
        case data = "data"
    }
    
    init(from decoder: Decoder) throws {
        //데이터로 들어오는 값이 없을수도 있고 있을수도 있어서 먼저 처리해주는 것
        //데이터가 없을 때 nil로 처리
        let values = try decoder.container(keyedBy: CodingKeys.self)
        message = (try? values.decode(String.self, forKey: .message)) ?? ""
        data = (try? values.decode(T.self, forKey: .data)) ?? nil
    }
}
```



## Data Model 만들기

> quicktype.io

### SignInData.swift

```swift
// MARK: - SignInData
struct SignInData: Codable {
    var email, password, userName: String
}
```

통신에 성공했을 때에 받아올 타입 생성



## 서버 통신에 따른 결과

### NetworkResult.swift

```swift
//서버 통신과의 성공, 실패 등을 처리해주기 위한 열거형(enum) 타입
// 서버 통신에 대한 결과(성공, 요청에러, 경로에러, 서버내부에러, 네트워크 연결 실패)
enum NetworkResult<T> {
    case success(T) //임의로 만든 데이터 T 를 담아서 보낼 수 있음
    case requestErr(T)
    case pathErr
    case serverErr
    case networkFail
}
```



## 통신 구현하기

### AuthService.swift

> 로그인 서버 통신을 위한 구조체

```swift
//로그인 서버 통신 구현을 위한 구조체
import Foundation
import Alamofire // 라이브러리 사용을 위해 import

struct AuthService { //파일 이름과 동일하게
    //싱글톤 디자인패턴 이용
    //싱글톤 객체 - 앱 어디서든 접근 가능
    static let shared = AuthService()
    
  	// 로그인 통신에 대한 함수 정의	
    	//closure를 함수의 파라미터로 받음
    	//@escaping - 탈출 클로저
    func signIn(email: String,
                password: String,
                completion: @escaping (NetworkResult<Any>) -> (Void)){
        
        let url = APIConstants.usersSignInURL // 통신 url
        let header: HTTPHeaders = [ // 요청 헤더: swift 딕셔너리형
            "Content-Type":"application/json",
          	"Authorization": UserDefaults.standard.string(forKey: "token") ?? ""
        ]
        let body: Parameters = [ // 요청 바디
            "email": email,
            "password":password
        ]
        
        // 원하는 형식의 HTTP Request 생성
        let dataRequest = AF.request(url,
                                     method: .post,
                                     parameters: body,
                                     encoding: URLEncoding.default, headers: header)
        
        // 데이터 통신 시작
        dataRequest.responseData{ (response) in // response = 통신의 결과
            // 통신 결과에 대한 분기 처리
            switch response.result {
            case .success:
              // 통신의 결과에 따라 statusCode와 value값을 가짐
                guard let statusCode = response.response?.statusCode else {
                    return
                }
                guard let data = response.value else {
                    return
                }
              // completion이란 클로져에게 전달할 데이터를 judgeSignInData라는 함수를 통해 결정
                completion(judgeSignInData(status: statusCode, data: data))
                
            case .failure(let err): print(err)
                completion(.networkFail) }
        }
    }
    
  // statusCode랑 decode 결과에 따라 NetworkResult를 반환시켜 줌
    private func judgeSignInData(status: Int, data: Data) -> NetworkResult<Any> {
      // 통신을 통해 전달받은 데이터를 decode
        let decoder = JSONDecoder()
        guard let decodedData = try? decoder.decode(GenericResponse<SignInData>.self, from: data) else {
            return .pathErr
        }
        // statusCode를 통해 통신 결과를 알 수 있음
        switch status {
          // 성공적으로 통신에 성공했다는 결과와 함께 decode한 data값도 전달
            case 200:
                return .success(decodedData.data)
          // 통신에는 성공했지만, 요청값에 대한 오류 처리.
          // 오류 결과와 함께 오류 메세지 전달
            case 400..<500:
                return .requestErr(decodedData.message)
          // server상의 에러 코드 (server 개발자가 지정)
          // 에러 결과만 보내 줌
            case 500:
                return .serverErr
            default:
                return .networkFail }
    }
    
}
```

> 싱글톤?
>
> 특정 용도로 객체를 하나 생성하여 공용으로 사용하고 싶을 때 사용하는 방법  
> 여러 객체에서 접근 가능하도록 데이터를 사용하는 것  
> 프로그램 내에서 단 하나의 인스턴스로만 클래스를 관리하고 사용할 수 있음  
> 그러나 생성되고 나면 프로그램 종료 시까지 항상 메모리에 올라가 있으므로 적절하게 사용해야 함

> @escaping 탈출 클로저
>
> non-escaping인 경우에는 해당 함수 내에서의 호출만 가능  
> 이러한 제약을 무시하기 위해 Escaping Closure를 사용함



## VC에 연결













