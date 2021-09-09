---
layout: post
title: "코스 라이브러리 뷰 리디자인 하기" 
date: 2021-09-09
category: read 
excerpt: ""
---

# 코스 라이브러리 뷰 리디자인 하기

> 사실 리디자인 이라는 용어를 쓰는게 맞는건지 모르겠음. "갈아엎기" 인데..

<img src="https://user-images.githubusercontent.com/28949235/132507397-68aedbc1-3a44-4e27-8a9c-6071788463ed.png" alt="image" width=200px /><img src="https://user-images.githubusercontent.com/28949235/132510802-63af2d8a-c482-4b33-9414-9363b3e05d15.png" alt="image" width=200 />

기존 뷰와 바뀐 뷰.  

### 변경사항 정리

![image](https://user-images.githubusercontent.com/28949235/132510934-287faa02-5518-468d-a6bb-04efff466616.png)

1️⃣ 상단에 코스 종류 선택 collection view가 추가됐다.

<img src="https://user-images.githubusercontent.com/28949235/132511058-3d94e4a0-1fb8-4b48-9013-9089246941e5.png" alt="image" width=300 /><img src="https://user-images.githubusercontent.com/28949235/132511070-18b46349-10df-4ac1-8b65-75943ac17c9c.png" alt="image" width=300 />

2️⃣ cell 디자인이 변경됐다.

<img src="https://user-images.githubusercontent.com/28949235/132511246-26e5e828-d2bf-4cf1-90fb-9996ed015751.png" alt="image" width=300 /><img src="https://user-images.githubusercontent.com/28949235/132511302-8c214c92-6132-4bbe-95be-50b5ba640722.png" alt="image" width=300 />

3️⃣ 완료한 코스를 보여주는 디자인이 변경됐다.

4️⃣ API response Data Model이 변경됐다.



하나하나 #해보자고



### Course Enum 만들기

여러 곳에서 코스 이름, 코스 테마 색 등이 사용되는데, 이를 한번에 관리할 Enum을 만들면 좋겠다고 생각했다.
localizing 고려해서도 이게 편할 듯 싶어서...

AppModel 폴더에 AppCourse.swift 를 만들어 준다.

![image](https://user-images.githubusercontent.com/28949235/132520470-67d1ab59-8a90-43ee-9515-7259d3a6b599.png)

코스 종류는 이렇게 7개다.

각 코스마다 달라지는 값들을 먼저 정리해보자면, 

1. 코스 한글 이름 (늘 건강행, 나 케어행 같은...) -> String

2. 코스 테마 색 -> UIColor

3. 앱 이곳저곳에서 사용하는 이미지 3개 -> UIImage

   ![image](https://user-images.githubusercontent.com/28949235/132521711-2c02408c-c940-4a17-b206-bff0952ae3f3.png)

4. 스탬프 이미지 2개 -> UIImage

   ![image](https://user-images.githubusercontent.com/28949235/132522327-2e4d0854-3a4a-4b11-bd4c-c8a687224d95.png)



각각 함수를 만들어 준다.

```swift
enum AppCourse: Int {
    // 늘 건강행, 나 케어행, 꼭 지켜야행, 꺅 일탈행, 호 추억행, 쪽 사랑행, 짱 똑똑행
    // 건강, 셀프케어, 습관, 도전, 추억, 사랑, 교양
    
    case health = 0, selfCare, habit, challenge, memory, love, culture
    
    static var count: Int { return AppCourse.culture.rawValue + 1}
    
    func getKorean() -> String {
        
        switch self {
        case .health:
            return "늘 건강행"
        case .selfCare:
            return "나 케어행"
        case .habit:
            return "꼭 지켜야행"
        case .challenge:
            return "꺅 일탈행"
        case .memory:
            return "호 추억행"
        case .love:
            return "쪽 사랑행"
        case .culture:
            return "짱 똑똑행"
        }
        
    }
    
    func getLightColor() -> UIColor {
    }
    
    func getDarkColor() -> UIColor {
    }
    
    func getBigImage() -> UIImage {
    }
    
    func getLibraryImage() -> UIImage {
    }
    
    func getSmallImage() -> UIImage {
    }
    
    func getUndoneStampImage() -> UIImage {
    }
    
    func getDoneStampImage() -> UIImage {
    }
}
```

그리고 안에 switch문으로 분기처리 해서 알맞은 것 return해주면 끝!

### 1️⃣ 상단에 코스 종류 선택 collection view 추가하기

![image](https://user-images.githubusercontent.com/28949235/132712484-2ca9209b-98de-4de1-8439-f8fff15053d7.png)

스토리보드에서 collection view를 추가해준다.

![image](https://user-images.githubusercontent.com/28949235/132712677-c86252ab-d176-4dc5-ae31-4190bdf110cb.png)

@IBOutlet 연결도 해 주고,

![image](https://user-images.githubusercontent.com/28949235/132713661-79207e26-a465-4268-be44-5671cc5663dc.png)

Cell 도 만들어 준다. (간단한 cell이라 그냥 SB에서 바로 작성했음)

Cell 연결해주고, 각각 올바른 데이터들을 넣어준다.

```swift
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        
        switch collectionView {
        case categoryCollectionView:
            if let cell = collectionView.dequeueReusableCell(withReuseIdentifier: Const.Xib.Identifier.courseCategoryCollectionViewCell, for: indexPath) as? CourseCategoryCollectionViewCell {
                
                if indexPath.row == 0 {
                    cell.setLabel(title: "전체")
                } else {
                    if let title = AppCourse(rawValue: indexPath.row-1)?.getKorean() {
                        cell.setLabel(title: title)
                    }
                }
                
                return cell
                
            }
```

```swift
    func numberOfSections(in collectionView: UICollectionView) -> Int {
        return courseListViewModel.numberOfSections
    }
    
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        
        switch collectionView {
        case categoryCollectionView:
            return AppCourse.count
            
        case courseLibraryCollectionView:
            return courseListViewModel.numberOfRowsInSection(0)
        default:
            return 0
        }
        
```

AppCourse Enum은 이런 식으로 활용했다.

![image](https://user-images.githubusercontent.com/28949235/132719855-2d10617e-4e3e-46c9-ab56-6c69bc42afce.png)

잘 뜬다

### 2️⃣ cell 디자인이 변경됐다.

![image](https://user-images.githubusercontent.com/28949235/132725191-94b1c64a-8023-4e99-bd72-37572f366cb7.png)

Xib에서 cell design을 변경해주고,
swift 파일에서 setCell 부분을 변경해준다.

![image-20210910013421295](/Users/choyi/Library/Application Support/typora-user-images/image-20210910013421295.png)

이만큼의 코드를 (코스 종류가 더 늘어났으니 원래는 더 길다)

![image](https://user-images.githubusercontent.com/28949235/132726350-2a956aca-8c9b-47f1-8063-e26af9825be9.png)

요렇게 깔끔 !!! 하게 줄일 수 있다.

### 3️⃣ 완료한 코스를 보여주는 디자인이 변경됐다.

Done Cell을 삭제하고, UndoneCell에 complete 여부에 따라 도장을 붙이고 안 붙이는 함수를 추가해 주면 된다.

Xcode의 refactor 기능을 사용해 UndoneCell을 CourseLibraryCollectionViewCell으로 renaming 해 줬다.

