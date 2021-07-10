---
layout: post
title: "Networking-5 : Moya" 
date: 2021-07-10
category: read 
excerpt: ""

---

# Networking 정리 5 : Moya

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

## Service 만들기

> ChallengeService.swift

```swift
import Moya
```

```swift
enum ChallengeService {
    // 전체 챌린지 지도 조회
    case getAllChallenges
    // 오늘의 챌린지 조회
    case getTodayChallenge(courseId: String, challengeId: String)
    // 챌린지 인증
    case putTodayChallenge(courseId: String, challengeId: String)
    // 완료한 코스 메달
    case getMedal
}
```

관련 API들을 enum으로 만들어준다.  
다음에 이 case들을 가지고 HTTPMethod, URL 등을 분기처리 할거라 만들어 주는 것~!

![image](https://user-images.githubusercontent.com/28949235/125157244-b86d7780-e1a4-11eb-99e9-81f8c7bc54d9.png)

그 다음에 위와 같이 TargetType을 작성해줄건데, 자동으로 에러가 뜨면서  
stubs를 추가할거냐고 묻는다. Fix를 눌러주면

![image](https://user-images.githubusercontent.com/28949235/125157245-be635880-e1a4-11eb-8de5-1ae0c2ffa5d5.png)

요렇게 stubs가 추가되고, 이제 요것들을 하나씩 채워주면 된다

### baseURL

```swift
var baseURL: URL {
        return URL(string: Const.URL.baseURL)!
    }
```

Const 파일에 만들어둔 baseURL을 URL으로 형변환해서 반환해준다.   
공통 URL을 baseURL으로 지정해준 후, 아래 path에 따라서 뒤에 각자 다르게 붙는 부분들을 분기처리 해준다.

### path

```swift
    var path: String {
        switch self {
        // 전체 챌린지 지도 조회
        case .getAllChallenges:
            return Const.URL.challengesURL
        // 오늘의 챌린지 조회
        case .getTodayChallenge(let courseId, let challengeId):
            return Const.URL.challengesURL + "/\(courseId)/\(challengeId)"
        // 챌린지 인증
        case .putTodayChallenge(let courseId, let challengeId):
            return Const.URL.challengesURL + "/\(courseId)/\(challengeId)"
        // 완료한 코스 메달
        case .getMedal:
            return Const.URL.medalURL

        }
    }
```

이렇게 아까 정의한 `ChallengeService` Enum에 따라 분기처리를 해주면 된다.  
baseURL 뒤에 붙을 것들을 각각 붙여주면 된다. 매개변수가 필요하면 필요한대로 작성!  
이때, `getTodayChallenge(courseId: String, challengeId: String)` 에서  
`.getTodayChallenge(let courseId, let challengeId)` 같은 형식으로 바꿔준 후  
`"/\(courseId)/\(challengeId)"` 이렇게 문자열 보간법 등을 사용해서 URL에 포함시켜주면 된다.

### method

```swift
    var method: Moya.Method {
        switch self {
        // 전체 챌린지 지도 조회
        case .getAllChallenges:
            return .get
        // 오늘의 챌린지 조회
        case .getTodayChallenge(_, _):
            return .get
        // 챌린지 인증
        case .putTodayChallenge(_, _):
            return .put
        // 완료한 코스 메달
        case .getMedal:
            return .get
        }
    }
```

HTTPMethod (GET, POST, PUT, DELETE) 를 분기처리해서 지정해준다.  
이게 바로 Moya의 큰 장점 중 하나 ~!!

### sampleData

```swift
    var sampleData: Data {
        // TODO: - Unit test 를 위한 Sample data 집어넣기

        // 임시 기본 sample data
        return Data()
    }
```

이건 Unit test를 할 때 네트워크 상황에 따라 테스트 결과가 달라지지 않도록 하기 위해  
sampleData를 넣어주는 과정인데, 우선 Data()를 반환해준다. (나중에 따로 포스팅 작성하겠음..!!)

### task

```swift
    var task: Task {
        switch self {
        // params가 없는 API - .requestPlain
        case .getAllChallenges, .getMedal, .getTodayChallenge(_, _), .putTodayChallenge(_, _):
            return .requestPlain
        }
    }
```

그다음 Task 종류를 지정해주는데, Moya에는 여러가지 Task가 있다.

![image](https://user-images.githubusercontent.com/28949235/125157884-721a1780-e1a8-11eb-9c83-a9ada6d19832.png)

몇개만 살펴보자면

* `.requestPlain` : 추가적인 data가 없는 요청
* `.requestParameters` : parameters를 넘겨서 요청시 body를 입력할 수 있음
* `.uploadFile` : 파일 업로드 요청

난 다 body가 없는 API로만 작업하고 있어서, 모든 경우에 `.requestPlain`을 반환해 줬다.

### headers

```swift
    var headers: [String: String]? {
        return [
            "Content-Type": "application/json",
            "Bearer": testToken
        ]
    }
```

넘겨줄 헤더를 작성하면 된다.  
이때도 분기처리가 필요하면 당연히 가능 !

**전체 코드**

```swift
//
//  ChallengeService.swift
//  Journey
//
//  Created by 초이 on 2021/07/10.
//

import Foundation
import Moya

let testToken = "테스트 jwt 토큰"

enum ChallengeService {
    // 전체 챌린지 지도 조회
    case getAllChallenges
    // 오늘의 챌린지 조회
    case getTodayChallenge(courseId: String, challengeId: String)
    // 챌린지 인증
    case putTodayChallenge(courseId: String, challengeId: String)
    // 완료한 코스 메달
    case getMedal
}

extension ChallengeService: TargetType {
    var baseURL: URL {
        return URL(string: Const.URL.baseURL)!
    }

    var path: String {
        switch self {
        // 전체 챌린지 지도 조회
        case .getAllChallenges:
            return Const.URL.challengesURL
        // 오늘의 챌린지 조회
        case .getTodayChallenge(let courseId, let challengeId):
            return Const.URL.challengesURL + "/\(courseId)/\(challengeId)"
        // 챌린지 인증
        case .putTodayChallenge(let courseId, let challengeId):
            return Const.URL.challengesURL + "/\(courseId)/\(challengeId)"
        // 완료한 코스 메달
        case .getMedal:
            return Const.URL.medalURL

        }
    }

    var method: Moya.Method {
        switch self {
        // 전체 챌린지 지도 조회
        case .getAllChallenges:
            return .get
        // 오늘의 챌린지 조회
        case .getTodayChallenge(_, _):
            return .get
        // 챌린지 인증
        case .putTodayChallenge(_, _):
            return .put
        // 완료한 코스 메달
        case .getMedal:
            return .get
        }
    }

    var sampleData: Data {
        // TODO: - Unit test 를 위한 Sample data 집어넣기

        // 임시 기본 sample data
        return Data()
    }

    var task: Task {
        switch self {
        // params가 없는 API - .requestPlain
        case .getAllChallenges, .getMedal, .getTodayChallenge(_, _), .putTodayChallenge(_, _):
            return .requestPlain
        }
    }

    var headers: [String: String]? {
        return [
            "Content-Type": "application/json",
            "Bearer": testToken
        ]
    }
}

```



### 통신 함수 만들기

> ChallengeAPI.swift (네이밍 진짜 맘에 안듦)

```swift
import Moya
```

```swift
public class ChallengeAPI {
}

```

API class를 만들어준다.

```swift
static let shared = ChallengeAPI()
```

여러 VC에서 같은 인스턴스에 접근해 사용할 수 있도록 싱글톤 인스턴스를 생성해준다.

```swift
var challengeProvider = MoyaProvider<ChallengeService>()
```

그리고 MoyaProvider를 만들어준다.

```swift
public init() { }
```

기본 init 함수도 만들어준다.

```swift
func getAllChallenges(completion: @escaping (NetworkResult<Any>) -> Void) {
        challengeProvider.request(.getAllChallenges) { (result) in
            
            switch result {
            case.success(let response):
                
                let statusCode = response.statusCode
                let data = response.data
                
                let networkResult = self.judgeStatus(by: statusCode, data)
                completion(networkResult)
                
            case .failure(let err):
                print(err)
            }
            
        }
    }
```

그다음에 통신 함수를 작성해줄건데,  
**@escaping**을 사용해서 매개변수로 넘어온 클로저를 밖으로 탈출시킬 수 있게 해준다.  
덕분에서버에서 데이터를 받아오는 작업이 끝났을 때 탈출 클로저를 호출하는 게 가능해짐!

우리가 Service에서 작성해둔 것 중 .getAllChallenges로 분기처리 해 준 것들을 종합해서 통신 request를 보낸다.  
그리고 그 결과로 받은 result에 따라서 .success, .failure로 분기처리를 해 준다. 

이때, 서버 통신을 성공하기만 하면 (status code가 200이든 500이든..) result값은 .success이다.  
진짜 서버 통신 자체가 안 될 때만 .failure가 뜨는 것!

그래서, .success인 경우에 내부에서 statusCode에 따라 세부적인 성공 실패 여부를 판단해야 한다.  
이를 위해 `judgeStatus()` 함수를 호출하는 것.



```swift
    private func judgeStatus(by statusCode: Int, _ data: Data) -> NetworkResult<Any> {
        switch statusCode {
        case 200:
            return isValidData(data: data)
        case 400..<500:
            return .pathErr
        case 500:
            return .serverErr
        default:
            return .networkFail
        }
    }
```

statusCode에 따라 NetworkResult를 반환해준다.  
근데 이때, 성공시에는 data를 decoding해서 같이 반환하기 위해 `isValidData`를 호출해준다.

```swift
    private func isValidData(data: Data) -> NetworkResult<Any> {
        let decoder = JSONDecoder()
        
        guard let decodedData = try? decoder.decode(CourseResponseData.self, from: data) else {
            return .pathErr
        }
        
        return .success(decodedData.data)
    }
```

요렇게!



**전체 코드**

```swift
//
//  ChallengeAPI.swift
//  Journey
//
//  Created by 초이 on 2021/07/10.
//

import Foundation
import Moya

public class ChallengeAPI {
    
    static let shared = ChallengeAPI()
    var challengeProvider = MoyaProvider<ChallengeService>()
    
    public init() { }
    
    func getAllChallenges(completion: @escaping (NetworkResult<Any>) -> Void) {
        challengeProvider.request(.getAllChallenges) { (result) in
            
            switch result {
            case.success(let response):
                
                let statusCode = response.statusCode
                let data = response.data
                
                let networkResult = self.judgeStatus(by: statusCode, data)
                completion(networkResult)
                
            case .failure(let err):
                print(err)
            }
            
        }
    }
    
    private func judgeStatus(by statusCode: Int, _ data: Data) -> NetworkResult<Any> {
        switch statusCode {
        case 200:
            return isValidData(data: data)
        case 400..<500:
            return .pathErr
        case 500:
            return .serverErr
        default:
            return .networkFail
        }
    }
    
    private func isValidData(data: Data) -> NetworkResult<Any> {
        let decoder = JSONDecoder()
        
        guard let decodedData = try? decoder.decode(CourseResponseData.self, from: data) else {
            return .pathErr
        }
        
        return .success(decodedData.data)
    }
}

```



## VC에서 사용하기

```swift
// MARK: - 서버 통신

extension CourseViewController {

    func getCourse() {
        
        ChallengeAPI.shared.getAllChallenges { (response) in
            
            switch response {
            case .success(let course):
                
                if let data = course as? CourseData {
                    self.updateData(data: data) // UI 등 할일 작성, reloadData 등등..
                }
            case .requestErr(let message):
                print("requestErr", message)
            case .pathErr:
                print(".pathErr")
            case .serverErr:
                print("serverErr")
            case .networkFail:
                print("networkFail")
            }
        }
    }
    
}
```

요렇게 간단하게 사용해줄 수 있다.  
받아온 NetworkResult값에 따라 분기처리만 해주면 끝~~



요건 승찬이오빠를 위한 그림... 이해 파이태잉!!

![제목_없는_아트워크 6](https://user-images.githubusercontent.com/28949235/125158855-a09af100-e1ae-11eb-8c7d-adb923390034.png)
