---

layout: post
title: "Moya parameters with same key" 
date: 2021-08-04
category: read 
excerpt: ""

---

## Moya parameters with same key

비어있다 서버를 붙이는데

```
http://localhost:8081/api/beers?min_abv=5&max_abv=6&beer_style=TEST_STYLE_1&aroma=TEST_AROMA_4&cursor=10&max_count=100&sort_by=review_count_desc
```

이렇게... 여러 정보를 param으로 받지만 모든게 다 Optional인 case가 있었드랬다...  

```swift
enum BeerListService {
    case getBeerList(minAbv: Int?, maxAbv: Int?, style: String?, scent: String?, cursor: Int?, maxCount: Int?, sort: Sort?)
}
```

그래서 저걸 보고 이렇게 case를 만들었었고,  
얘가 있는지 없는지 ( nil인지 아닌지 ) 판단해서 params로 URL에 붙여줘야 했다.

params를 쓰니까 task는 `.requestParameters` 일거고,  
`.requestParameters` 는

```swift
.requestParameters(parameters: <#T##[String : Any]#>, encoding: <#T##ParameterEncoding#>)
```

이렇게 생겼기 때문에 params를 [String : Any], 즉 딕셔너리 타입으로 넘겨주면 된다.



```swift
var task: Task {
        switch self {
        case .getBeerList(let minAbv, let maxAbv, let style, let scent, let cursor, let maxCount, let sort):
            
            var params: [String:Any] = [:]
            
            if let minAbv = minAbv {
                params["min_abv"] = minAbv
            }
            
            if let maxAbv = maxAbv {
                params["max_abv"] = maxAbv
            }
            
            if let beerStyle = style {
                params["beer_style"] = beerStyle
            }
            
            if let aroma = scent {
                params["aroma"] = aroma
            }
            
            if let cursor = cursor {
                params["cursor"] = cursor
            }
            
            if let maxCount = maxCount {
                params["max_count"] = maxCount
            }
            
            if let sort = sort {
                params["sort_by"] = sort
            }
        
            return .requestParameters(parameters: params, encoding: URLEncoding.queryString)
        }
    }
```

그래서 이렇게 각 매개변수의 nil 여부를 판단한 후, 키값이랑 value값을 잘 넣어줬다.  
(그럼 params만 보내면 알아서 key-value params로 다 넣어준다)  
(**encoding: URLEncoding.queryString** 이렇게 해주면 !!!)

근데 문제가 생겼다.,,, 

```swift
aroma, county, beer_style 등의 key를 중복 사용해서 해당 조건을 or로 걸 수 있습니다. 위 url에서는 beer_style이 중복 사용
```

중복으로 조건을 걸 수 있던거임...  
근데 dictionary의 key값은 unique해야 해서 여러개의 `params["beer_style"]` 을 만들 수 없었다. 

그럼 어떡하냐,, 그냥 String 말고 [String] 으로 넣어주면 알아서 잘 들어가더라..

```swift
enum BeerListService {
    case getBeerList(minAbv: Int?, maxAbv: Int?, style: [String]?, scent: [String]?, cursor: Int?, maxCount: Int?, sort: Sort?)
}
```

이렇게만 넘겨주면 string 배열의 style 변수 내에 있는 item들을 하나하나 query로 붙여서 날려준다.  
편하고만,,~~

