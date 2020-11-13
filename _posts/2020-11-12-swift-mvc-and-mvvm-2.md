---
layout: post
title: "Swift MVC 패턴 되돌아보기" 
date: 2020-11-12
category: read 
excerpt: ""

---

# Swift MVC 패턴 되돌아보기

[이 글](https://iamcho2.github.io/2020/11/11/swift-mvc-and-mvvm)과 이어지는 글

MVVM 통신 코드를 보기 전에,  
내가 그동안 어떻게 MVC 패턴을 적용해서 통신을 해 왔는지 부터 되돌아보자..

> 아래는 SOPT 24기 앱잼을 진행하면서 정리해놓은 통신 문서에 말을 덧붙인 것



사용한 라이브러리들

> **Alamofire**: Swift기반 HTTP 통신 라이브러리  
> **ObjectMapper**:  JSON 응답을 모델 객체로, 또는 그 반대로 변환. 주로 JSON을 객체에 매핑할 때 사용  
> **AlamofireObejctMapper**: ObjectMapper를 사용해서 자동으로  
> JSON response data --> swift 객체 변환해주는 Alamofire의 확장 라이브러리

1. Models 폴더에 <데이터>.swift 파일을 만든다

   > ex) NewArchive.swift

2. NewArchive.swift 내에 서버에서 주는 형식대로 Codable(또는 Mappable) 구조체를 만든다

   > 서버에서 주는 형식 : git wiki 확인  
   > quicktype.io를 사용하면 더 쉬움

   ```swift
   // 서버의 success response
   {
       "status": 200,
       "success": true,
       "message": "홈 신규 아티클/아카이브 조회 성공",
       "data": [
            {
               "archive_idx": 1,
               "user_idx": 4,
               "archive_title": "[UI/UX]당장 적용해 볼 수 있는 UI/UX 사례를 알고 싶습니다.",
               "date": "2019-06-30T15:00:00.000Z",
               "archive_img": "KakaoTalk_20190118_143330107_02.jpg",
               "category_idx": 6,
               "article_cnt": 13,
               "category_all": [
                   {
                       "category_title": "Design"
                   },
                   {
                       "category_title": "Plan"
                   }
               ]
           }
       ]
   }
   ```

   ```swift
   // NewArchive.swift
   struct NewArchive: Codable  {
       let archive_idx: Int
       let user_idx: Int
       let archive_title: String
       let date: String
       let archive_img: String
       let category_idx: Int
       let article_cnt: Int
       let category_all: [CategoryAll]
   }
   ```

   ```swift
   // Mappable : ObjectMapper 내 프로토콜
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

   > 구조체나 열거형에서 정의된 메소드가  
   > 자기 자신의 인스턴스를 수정하거나 프로퍼티를 변경해야 할 때 mutating 키워드를 사용
   >
   > 위 코드는 mapping func 이 자기 자신의 프로퍼티들을 변경하기 때문에  
   > 해당 func 에 mutating 키워드가 추가된 경우

3.  ResponseArray.swift 파일 만들기 (있으면 안해도 됨)

   ```swift
   struct ResponseArray<T: Codable>: Codable {
       let status: Int
       let success: Bool
       let message: String
       let data: [T]
   }
   ```

   ```swift
   import ObjectMapper
   
   struct ResponseArray<T: Mappable>: Mappable {
       
       var status: Int?
       var success: Bool?
       var message: String?
       var data: [T]?
       
       init?(map: Map) {}
       
       mutating func mapping(map: Map) {
           status <- map["status"]
           success <- map["success"]
           message <- map["message"]
           data <- map["data"]
       }
   }
   
   ```

4. NetworkResult.swift 파일 만들기 (있으면 안해도 됨)

   ```swift
   enum NetworkResult<T> {
       // 통신의 상태에 대한 분기 코드입니다.
       case success(T)
       case requestErr(T)
       case pathErr
       case serverErr
       case networkFail
   }
   ```

5. Model 폴더에 APIConstant.swift에 주소 추가하기 (있으면 안해도 됨)

   ```swift
   struct APIConstants {
       static let BaseURL = "http://11.111.11.111:3000"
       
       static let AuthURL = BaseURL + "/auth"
       static let SignupURL = AuthURL + "/signup"
       static let LoginURL = AuthURL + "/signin"
       
       static let SearchURL = BaseURL + "/search"
       
       static let HomeURL = BaseURL + "/home"
       static let HomeArtiURL = HomeURL + "/article"
       
     	// .. 이런 식 ..
     
       static let ArchiveScrapURL = BaseURL + "/mypage/archive/scrap"
   }
   ```

   > 아니면 APIManager.swift 파일을 만들어서 아래처럼 하는 방법도 있음
   >
   > ```swift
   > // APIManager.swift
   > protocol APIManager {}
   > 
   > extension APIManager {
   >     static func url(_ path: String) -> String {
   >         return "http://hyunjkluz.ml:2424/api" + path
   >     }
   > }
   > 
   > ```
   >
   > ```swift
   > //api service swift file
   > let MainURL = url("/webtoons/main")
   > ```

   

6. APIServices 폴더에 api service swift 파일 만들기 (있으면 안해두 됨)

   ```swift
   import Foundation
   import Alamofire
   
   struct NewArchiveService {
       
       static let shared = NewArchiveService()
       
       // App Auth API
       func getNewArchive(completion: @escaping (NetworkResult<Any>) -> Void) {
           
           let URL = "http://15.164.11.203:3000/home/archive/archives/new"
           
           let header: HTTPHeaders = [
               "Content-Type" : "application/json"
           ]
           
           Alamofire.request(URL, method: .get, parameters: nil, encoding: JSONEncoding.default, headers: header)
               .responseData { response in
                   
                   switch response.result {
                       
                   case .success:
                       if let value = response.result.value {
                           if let status = response.response?.statusCode {
                               
                               switch status {
                               case 200:
                                   do {
                                       print("do")
                                       print(value)
                                       
                                       let decoder = JSONDecoder()
                                       let result = try decoder.decode(ResponseArray<NewArchive>.self, from: value)
                                       
                                       print("try")
                                       print(result)
                                       
                                       switch result.success {
                                       case true:
                                           completion(.success(result.data))
                                       case false:
                                           completion(.requestErr(result.message))
                                       }
                                   } catch {
                                       print(".pathErr catch")
                                       completion(.pathErr)
                                   }
                               case 400:
                                   print(".pathErr 400")
                                   completion(.pathErr)
                               case 500:
                                   completion(.serverErr)
                                   
                               default:
                                   break
                               }
                           }
                       }
                       break
                       
                   case .failure(let err):
                       print(err.localizedDescription)
                       completion(.networkFail)
                       break
                   }
           }
       }
   }
   ```

   ```swift
   // Requestable 프로토콜을 만들어도 됨 - 이게 더 나아보임
   // Requestable.swift
   import Foundation
   import Alamofire
   import AlamofireObjectMapper
   import ObjectMapper
   
   //Request 함수를 재사용하기 위한 프로토콜
   protocol Requestable {
       associatedtype NetworkData: Mappable
   }
   
   extension Requestable {
       
       //서버에 get 요청을 보내는 함수
       func gettable(_ url: String, body: [String:Any]?, header: HTTPHeaders?, completion: @escaping (NetworkResult<NetworkData>) -> Void) {
           Alamofire.request(url, method: .get, parameters: body, encoding: JSONEncoding.default, headers: header)
               .validate(contentType: ["application/json"])
               .responseObject { (res: DataResponse<NetworkData>) in
                   switch res.result {
                   case .success:
                       guard let value = res.result.value else { return }
                       completion(.success(value))
                   case .failure(let err):
                       completion(.error(err))
                   }
           }
       }
       
       //서버에 post 요청을 보내는 함수
       func postable(_ url: String, body: [String:Any]?, header: HTTPHeaders?, completion: @escaping (NetworkResult<NetworkData>) -> Void) {
           Alamofire.request(url, method: .post, parameters: body, encoding: JSONEncoding.default, headers: header)
               .validate(contentType: ["application/json"])
               .responseObject { (res: DataResponse<NetworkData>) in
                   switch res.result {
                   case .success:
                       guard let value = res.result.value else { return }
                       completion(.success(value))
                   case .failure(let err):
                       completion(.error(err))
                   }
           }
       }
       
       func putable(_ url: String, body: [String:Any]?, header: HTTPHeaders?, completion: @escaping (NetworkResult<NetworkData>) -> Void) {
           Alamofire.request(url, method: .put, parameters: body, encoding: JSONEncoding.default, headers: header)
               .validate(contentType: ["application/json"])
               .responseObject { (res: DataResponse<NetworkData>) in
                   switch res.result {
                   case .success:
                       guard let value = res.result.value else { return }
                       completion(.success(value))
                   case .failure(let err):
                       completion(.error(err))
                   }
               }
       }
       
       func delete(_ url: String, body: [String:Any]?, header: HTTPHeaders?, completion: @escaping (NetworkResult<NetworkData>) -> Void) {
           Alamofire.request(url, method: .delete, parameters: body, encoding: JSONEncoding.default, headers: header)
               .validate(contentType: ["application/json"])
               .responseObject { (res: DataResponse<NetworkData>) in
                   switch res.result {
                   case .success:
                       guard let value = res.result.value else { return }
                       completion(.success(value))
                   case .failure(let err):
                       completion(.error(err))
                   }
               }
       }
   }
   
   
   //MainService.swift
   import Alamofire
   
   struct MainService: APIManager, Requestable {
       
       typealias NetworkData = ResponseArray<Soptoon>
       static let shared = MainService()
       let MainURL = url("/webtoons/main")
       let headers: HTTPHeaders = [
           "Content-Type" : "application/json"
       ]
       
       // get soptoon list API
       func getSoptoon(flag: Int, completion: @escaping ([Soptoon]) -> Void) {
           
           let queryURL = MainURL + "/\(flag)"
           
           gettable(queryURL, body: nil, header: headers) { res in
               switch res {
               case .success(let value):
                   
                   print("######### success #########")
                   print(value)
                   print("######### success #########")
                   
                   guard let soptoonList = value.data else { return }
                   
                   completion(soptoonList)
               case .error(let error):
                   
                   print("######### error #########")
                   print(error)
                   print("######### error #########")
               }
           }
       }
   }
   ```

   

   7. 데이터를 사용할 swift 파일 (ViewController 같은) 에 변수 선언하기

   ```swift
   var newArchiveList: [NewArchive] = []
   ```

   8. 7번과 같은 파일에 함수 추가하기

   ```swift
   func getNewArchive() {
           
           NewArchiveService.shared.getNewArchive() {
               /*
                clusure 의 선언부에 [weak self] 를 명시해주고
                self 가 사용되는 곳에 self 를 옵셔널로 사용해주면
                strong reference cycle 을 피할 수 있다.
                
                어떠한 상황에서 해당 issue 가 발생하는 지 모르겠다면
                closure 내부에서 self 를 사용하는 경우
                [weak self] param in 을 항상 명시해주는 습관을 기르면 좋을 것이다.
               */
               [weak self]
               (data) in
               /*
                예약어의 경우 변수 이름을 grave accent 로 감싸주면
                변수로써 사용할 수 있다.
                
                이 곳에서 self 를 옵셔널로 사용해준 모습이다.
               */
               guard let `self` = self else { return }
               
               switch data {
                   
               case .success(let result):
                   let _result = result as! [NewArchive]
                   self.newArchiveList = _result
                   self.newArchiveCV.reloadData()
                   print(result)
                   
               case .requestErr(let message):
                   print(message)
               case .pathErr:
                   print("pathErr")
               case .serverErr:
                   print("serverErr")
               case .networkFail:
                   print("networkFail")
               }
           }
       }
   ```

   

   9. 사용

   ```swift
   let newArchive = newArchiveList[indexPath.row]
           cell.articleTitle.text = "\(newArchive.archive_title)"
   ```

   

