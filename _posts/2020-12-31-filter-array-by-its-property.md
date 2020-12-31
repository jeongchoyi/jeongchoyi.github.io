---
layout: post
title: "특정 프로퍼티에 따라 배열 나누기" 
date: 2020-12-30
category: read 
excerpt: ""

---

# 특정 프로퍼티에 따라 배열 나누기

### 데이터 모델

```swift
struct Bubble {
    var date: String
    var cate: String
    var depth: Int //0~6
    var leadingNum: Int //0~9
}
```

### 더미데이터 배열

```swift
struct BubbleData {
    var objectsArray = [
        Bubble(date: "12.1", cate: "0사랑", depth: 0, leadingNum: 0),
        Bubble(date: "12.10", cate: "0추억", depth: 0, leadingNum: 4),
        Bubble(date: "12.27", cate: "0행복", depth: 0, leadingNum: 7),
        Bubble(date: "12.31", cate: "0사랑", depth: 0, leadingNum: 9),
        Bubble(date: "12.12", cate: "1사랑", depth: 1, leadingNum: 5),
        Bubble(date: "12.6", cate: "1슬픔", depth: 1, leadingNum: 6), // 아 여기 잘못했네
        Bubble(date: "12.5", cate: "2화남", depth: 2, leadingNum: 5),
        Bubble(date: "12.9", cate: "3우울", depth: 3, leadingNum: 1),
        Bubble(date: "12.3", cate: "4위로", depth: 4, leadingNum: 2),
        Bubble(date: "12.8", cate: "4사랑", depth: 4, leadingNum: 2),
        Bubble(date: "12.4", cate: "5일상", depth: 5, leadingNum: 4),
        Bubble(date: "12.7", cate: "6행복", depth: 6, leadingNum: 7),
    ]
}
```

근데 single data array로 multiple section에 데이터 뿌리는 건 좋지 않은 방법이라  
배열을 단계별로 분리해야 된다.

### 배열 depth별로 분리하기

```swift
func devideArrayByDepth(){
    let totalArray = bubbleDataArray.objectsArray
    for sectionIndex in 0..<tableView.numberOfSections {
        let bubbleArray = totalArray.filter { (bubble : Bubble) -> Bool in
            return bubble.depth == sectionIndex
        }
        bubbleDepthArray.append(bubbleArray)
    }
}
```

이렇게 filter 하고 결과물을 append해주면

```swift
[[momo_home_practice.Bubble(date: "12.1", cate: "0사랑", depth: 0, leadingNum: 0),
 momo_home_practice.Bubble(date: "12.10", cate: "0추억", depth: 0, leadingNum: 4),
 momo_home_practice.Bubble(date: "12.27", cate: "0행복", depth: 0, leadingNum: 7),
 momo_home_practice.Bubble(date: "12.31", cate: "0사랑", depth: 0, leadingNum: 9)],
 momo_home_practice.Bubble(date: "12.6", cate: "1슬픔", depth: 1, leadingNum: 6)],
 [momo_home_practice.Bubble(date: "12.12", cate: "1사랑", depth: 1, leadingNum: 5),
 [momo_home_practice.Bubble(date: "12.5", cate: "2화남", depth: 2, leadingNum: 5)],
 [momo_home_practice.Bubble(date: "12.9", cate: "3우울", depth: 3, leadingNum: 1)],
 [momo_home_practice.Bubble(date: "12.3", cate: "4위로", depth: 4, leadingNum: 2),
 momo_home_practice.Bubble(date: "12.8", cate: "4사랑", depth: 4, leadingNum: 2)],
 [momo_home_practice.Bubble(date: "12.4", cate: "5일상", depth: 5, leadingNum: 4)],
 [momo_home_practice.Bubble(date: "12.7", cate: "6행복", depth: 6, leadingNum: 7)]]
```



잘나와요잉