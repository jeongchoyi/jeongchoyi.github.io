---
layout: post
title: "SwiftUI : List-ForEach" 
date: 2020-11-6
category: read 
excerpt: ""

---

# SwiftUI : **`List `,`ForEach`**

## List - ForEach

> 테이블 뷰 처럼 쭉 나오는 형태

List가 있으려면 데이터가 있어야 하는데, 그런 Data Model을 직접 만든다고 했을 때 Model을 만드는 것 까지 배워보자

```swift
struct LocationInfo {
	var city = ""
	var weather = ""
}
```

이런 model을 가지고, 왼쪽에는 도시 이름, 오른쪽엔 날씨 정보가 나오는 List를 만들어보자

```swift
@State private var locations = [
	locationInfo(city: "seoul", weather: "rainy"),
	locationInfo(city: "daejeon", weather: "sunny"),
  locationInfo(city: "busan", weather: "cloudy")
]
List{
	HStack{ //하나의 row (cell)
	
	}
}
```

* Data를 List에 녹이기 위해선 For문 - ForEach문을 많이 사용한다

* ForEach 요구조건이 여러 개 있는데 그 중에 상황에 따라 맞는 거 쓰면 됨

  * 1. **id**: 어떤 걸 아이디로 쓸래? city? weather? / **content**: 클로저형태

    * 근데 city나 weather나 같은 값이 다른 데이터에 들어갈 수 있어서 id로는 적절하지 않기 때문에 이 방식 적절X

    * id라는 개념이 필요한 이유 : 하나하나 셀을 구별하기 위해서

    * `locationInfo(city: "seoul", weather: "rainy")` 이런 거 하나하나를 고유한 id로서 쓰겠다 -> **id: \\.self** 라고 쓰면 됨

      * 이 때는 **Hashable** 프로토콜을 준수해야 함

        ```swift
        struct LocationInfo: Hashable {
        	var city = ""
        	var weather = ""
        }
        ```

    * content:는 클로저형태

      ```swift
      ForEach(locations, id: \.self) {location in 
        Hstack {
          Text("\(location.city)")
          Text("\(location.weather)")
        }
      }
      ```

      ![image](https://user-images.githubusercontent.com/28949235/98381495-4e3c8a00-208d-11eb-88c0-b206700bba85.png)

      <br>

  * 2. id가 없고 **content:**만 있는 형태

    ```swift
    ForEach(locations) {location in //위에도 그렇고 여기서의 location은 그냥 맘대로 정한 것
      Hstack {
        Text("\(location.city)")
        Text("\(location.weather)")
      }
    }
    ```

    * 얘처럼 id가 없는데 데이터를 뿌리려면, 모델이 **Identifiable** 프로토콜을 준수해야 함 - 필수구현 id값 하나 생성해야 함

      ```swift
      struct LocationInfo: Identifiable {
        var id = UUID() // 난수 고유한 값이 자동으로 생성되게 UUID()라고 쓴 
        
      	var city = ""
      	var weather = ""
      }
      ```

    * 1번에서 `id: \.id` 라고 쓴 것과 동일함! 2번은 그걸 생략한거

    ![image](https://user-images.githubusercontent.com/28949235/98382987-4251c780-208f-11eb-95bb-9a983593eaf8.png)

  * 3. 인덱싱 형태 - **Range\<Int>** 부분이 있는 형태

    * 0...10 = 0 ~ 10 / 0..<10 = 0 ~ 9

      ```swift
      ForEach(0..<locations.count) { index in //index는 앞의 숫자값을 받아옴 (이름은 임의로 정한 것)
       HStack{
         Text("\(index + 1)")
         Text("\(locations[index].city)")
         Text("\(locations[index].weather)")
       }
      }
      ```

    * 인덱스 자체로 뭘 하고싶을 때 사용

    ![image](https://user-images.githubusercontent.com/28949235/98382996-454cb800-208f-11eb-9304-05363d17b2d9.png)

