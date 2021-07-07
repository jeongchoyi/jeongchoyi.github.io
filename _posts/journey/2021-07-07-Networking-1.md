---
layout: post
title: "Networking-1 : Codable" 
date: 2021-07-06
category: read 
excerpt: ""

---

# Networking 정리 1 : Codable

### HTTP 프로토콜 메서드

* GET - 데이터를 얻고 싶을 때
* HEAD - 헤더 정보를 얻고 싶을 때
* POST - 내용을 전송할 때
* PUT - 내용을 갱신하고 싶을 때
* DELETE - 내용을 삭제하고 싶을 때
* OPTIONS - 가능한 메소드 옵션을 파악할 때
* TRACE - 리소스가 수신되는 경로를 파악할 때
* PATCH - 리소스를 일부 수정할 때

### REST API

> Representational State Transfer API

* 행위 (HTTP 메서드 중에서 GET/POST/PUT/DELETE 사용)
* 자원 (URI)
* 메세지 (JSON)

### Codable (Encodable과 Decodable을 묶은 것)

* Swift Data Model --> JSON : Encode

  * Encodable이라는 프로토콜을 채택해야 함 ( 채택 = 해당 데이터 모델을 다른 데이터로 인코딩이 가능하다는 뜻 )

    ```swift
    struct PersonDataModel: Encodable {
      var name: String
      var age: Int
    }
    // ...
    let personData = PersonDataModel(name: "철수", age: 10)
    // ...
    let jsonEncoder = JSONEncoder()
    jsonEncoder.outputFormatting = .prettyPrinted
    
    do {
      let data = try jsonEncoder.encode(personData)
      print(String(data: data, encoding: .utf8)!)
    } catch {
      print(error)
    }
    ```

    

*  JSON --> Swift Data Model : Decode

  * Decodable이라는 프로토콜을 채택해야 함 ( 채택 = 해당 데이터 모델은 다른 데이터에서부터 요 데이터 모델로 디코딩이 가능하다는 뜻)

    ```swift
    struct CoffeeDataModel: Decodable {
      var drink: String,
      var price: Int,
      var orderer: String
    }
    // ...
    let dummyData = """
    {
      "drink" : "아메리카노",
      "price" : "2000",
      "orderer" : "JH"
    }
    """.data(using: .utf8)
    //...
    let jsonDecoder = JSONDecoder()
    do {
      let order = try jsonDecoder.decode(CoffeeDataModel.self, from: dummyData)
      print("디코더 성공")
      dump(order) // 자세한 정보 출력
    } catch {
      print(error)
    }
    ```

### Decodable 시 유의할 점

**1. Key의 이름이 바뀌었을 때**

Model을 아래와 같이 만들었는데,

```swift
struct CoffeeDataModel: Decodable {
  var drink: String,
  var price: Int,
  var orderer: String
}
```



```swift
{
  "drink" : "아메리카노",
  "price" : "2000",
  "orderer" : "JH"
}
```

서버에서 위 처럼 안 주고

```swift
{
  "drink" : "아메리카노",
  "coffee_price" : "2000",
  "orderer" : "JH"
}
```

이런 식으로 줬을 때 (price -> coffee_price) Model과 key값이 일치하지 않게 됨

**해결 : CodingKeys 사용**

```swift
struct CoffeeDataModel: Decodable {
  var drink: String,
  var price: Int,
  var orderer: String
  
  
  enum CodingKeys: String, CodingKey {
    case drink
    case price = "coffee_price"
    case orderer
  }
}
```

key 값 이름이 변경될 때, CodingKeys만 변경해주면 정상적으로 decoding됨. (접근은 그대로 price로 하면 됨!)



**2. Key-Value 한(또는 여러) 쌍이 없을 때**

Model을 아래와 같이 만들었는데,

```swift
struct CoffeeDataModel: Decodable {
  var drink: String,
  var price: Int,
  var orderer: String
}
```



```swift
{
  "drink" : "아메리카노",
  "price" : "2000",
  "orderer" : "JH"
}
```

서버에서 위 처럼 안 주고

```swift
{
  "drink" : "아메리카노",
  													// 이 부분을 안 줌
  "orderer" : "JH"
}
```

이렇게 줬을 때, key-value 한 쌍이 존재하지 않아서 전체 decode가 한번에 안 됨.

**해결 : 직접 decode 하기**

```swift
struct CoffeeDataModel: Decodable {
  var drink: String,
  var price: Int,
  var orderer: String
  
  
  enum CodingKeys: String, CodingKey {
    case drink
    case price = "coffee_price"
    case orderer
  }
  
  init(from decoder: Decoder) throws {
    let values = try decoder.container(keydBy: CodingKeys.self) {
      drink = (try? values.decode(String.self, forKey: .drink)) ?? ""
      price = (try? values.decode(String.self, forKey: .price)) ?? 0
      orderer = (try? values.decode(String.self, forKey: .orderer)) ?? ""
    }
  }
}
```

Model에다가 init 함수를 작성해서 직접 decode 부분을 구현해 줌.  
이렇게 하면 한 쌍이 없더라도 기본값으로 정해둔 값 ("" / 0 등...)이 들어감.

