---
layout: post
title: "tableview cell 안의 collectionview" 
date: 2021-05-17
category: read 
excerpt: ""

---

# tableview cell 안의 collectionview

## 1. UI짜기

![image](https://user-images.githubusercontent.com/28949235/118478164-84e51280-b74a-11eb-8a2a-e10e01e13e25.png)

요거처럼 tableview가 있고, 각 tableview cell 내에서 collectionview를 사용할 때,  
데이터 전달, 클릭 이벤트 트리거 처리하기 !! 

굉..~장히 많이 쓰이는 UI인데, 데이터 전달 부분이 좀 복잡하고 헷갈려서 ... 이번 기회에 마스터 해보려고 한다 아좌좌

### 뷰 설계

![Untitled_Artwork 4](https://user-images.githubusercontent.com/28949235/118484581-59662600-b752-11eb-9f4e-4ae48441e94b.png)

요 뷰를 짜볼건데, 우선! 제일 바깥에 있는 메인 뷰 부터 짜보자

### 메인 뷰

![image](https://user-images.githubusercontent.com/28949235/118484817-9c27fe00-b752-11eb-9ec0-9ef505e33ce7.png)

segmented control 커스텀하는 건 다른 포스트에서 하기로 하고, 이번에는 tableview에 집중할 것,,

![image](https://user-images.githubusercontent.com/28949235/118484892-b530af00-b752-11eb-9f0e-3f97f5ff07f9.png)

@IBOutlet 연결도 해주기!

### Cell Xib 만들기

이번에 만들어야 할 cell이 총 3개 있었는데, SOPT 클-디 합동세미나 시간에  
팀원들끼리 각자 한 개씩 맡아서 cell을 짰다.

<img src="https://user-images.githubusercontent.com/28949235/118485109-f3c66980-b752-11eb-8d56-bfa45fdd4797.png" alt="image" style="width:200px;" /><img src="https://user-images.githubusercontent.com/28949235/118485117-f7f28700-b752-11eb-9dd9-75b5d54361e6.png" alt="image" style="width:300px;" />

collectionView cell들은 나와있으니, tableView cell을 짜 봅시다 !  
뷰 설계한 이미지에서 위에서부터 차례대로~.~ (재사용할 수 있는 것들은 제외)

**첫번째 tableview cell** - `EditorPickTableViewCell`

<img src="https://user-images.githubusercontent.com/28949235/118485531-849d4500-b753-11eb-99a0-eb3e8a2ffe6d.png" alt="image" style="width:300px" /><img src="https://user-images.githubusercontent.com/28949235/118486245-5b30e900-b754-11eb-9ff8-ce54c70effe3.png" alt="image" style="width:300px" />

**두번째 tableview cell** - `ProjectsTableViewCell`

<img src="https://user-images.githubusercontent.com/28949235/118487003-3db04f00-b755-11eb-9cce-9d79eacb0fab.png" alt="image" style="width:300px" /><img src="https://user-images.githubusercontent.com/28949235/118486955-2d986f80-b755-11eb-9c20-8de1af29fd7a.png" alt="image" style="width:300px" />

**세번째 tableview cell** - `ExhibitionTableViewCell`

<img src="https://user-images.githubusercontent.com/28949235/118487165-67697600-b755-11eb-8b86-e7eb83f61977.png" alt="image" style="width:300px" /><img src="https://user-images.githubusercontent.com/28949235/118487734-fa0a1500-b755-11eb-887e-d455d383cd5f.png" alt="image" style="width:300px" />

이 밑에부턴 다 `ProjectsTableViewCell` 재사용!  
만들면서 각 셀 swift 파일에 @IBOutlet 연결도 다 해줬다.

## 2. Model 만들기

모델은 총 두개만 만들면 된다!

```swift
struct ProjectModel {
    var image: String
    var category: String
    var company: String
    var title: String
    var percentage: String
}
```

```swift
struct ExhibitionModel {
    var image: String
    var title: String
    var projectCount: String
    var tags: [String]
}
```

> 이미지는 나중에 통신으로 url 받아오면 `UIImage(data: )` 써서 UIImage로 만들면 됨!  
> 임시로 데이터 통신할 땐 named로 하고,,~

이때 tableview에 쓸 모델도 만들어서 section name 등을 관리해도 되지만,,  
어차피 table view row 개수도 5개고,  
다음 합동 세미나에서 통신할거 생각하면 그냥 이렇게 하는게 더 편할 것 같다는 생각..~~~!!

## 3. xib register 해주기

![image](https://user-images.githubusercontent.com/28949235/118491913-6be45d80-b75a-11eb-89f5-0a939b0bf97b.png)

## 4. tableview delegate, datasource 세팅하기

![image](https://user-images.githubusercontent.com/28949235/118492782-558ad180-b75b-11eb-9487-2e536a8e8133.png)

row 수 지정, cell은 indexPath.row로 switch 분기처리해서 지정 !  
if let 구문으로 cell을 만들고 나서 각 cell 내의 set cell 함수를 사용해서 데이터를 전달해 줄 예정이다. (아직 안 짬)

## 5. 각 table view cell 세팅하기

**첫번째 tableview cell** - `EditorPickTableViewCell`

`assignDelegate()` , `assignDataSource()`  , `registerXib()` 다 해주고

**Extension: UICollectionViewDelegateFlowLayout**

* sizeForItemAt
* minimumLineSpacingForSectionAt
* minimumInteritemSpacingForSectionAt
* insetForSectionAt

**Extension: UICollectionViewDataSource**

* numberOfItemsInSection
* cellForItemAt

까지 끝 !!

## 6. Sample Data 만들기

**데이터 구조 설계하기**

![Untitled_Artwork 6](https://user-images.githubusercontent.com/28949235/118602149-49e7eb00-b7ed-11eb-9249-9d4bc4599f66.png)

**샘플 데이터 입력 노가다**

![image](https://user-images.githubusercontent.com/28949235/118602258-684de680-b7ed-11eb-9fb0-21900b9dd9d3.png)

다..했어요.. ㅇ<-<

이제 데이터를 뿌려뿌려..~~

## 7. 데이터 뿌리기

우선 전체 Tableview로 가서,  

```swift
    // MARK: - Properties
    
    var exampleArray = Tumblebug()
```

exampleArray를 만들어준다. 그리고  `// set cell 함수 호출` 같은 식으로 주석처리 해 둔 곳에 가서,  

```swift
let rowArray = exampleArray.objectArray[indexPath.row]
```

rowArray를 각 case문마다 만들어주고 넘겨줄건데,  
각 tableview cell swift 파일에 가서 set cell 함수를 만들어준다.

rowArray 안의 element들로 tableview cell 내의 collectionview cell을 갱신할 거니까, extension에 작성한다.

```swift
class EditorPickTableViewCell: UITableViewCell {
    
    // MARK: - Properties
    
    var editorPickProjects: [ProjectModel]?
```

변수 만들어주고

```swift
// MARK: - UICollectionViewDataSource

extension EditorPickTableViewCell: UICollectionViewDataSource {
    
    func setCell(row: [ProjectModel]) {
        self.editorPickProjects = row
        self.editorPickCollectionView.reloadData()
    }
```

setCell 함수도 만들어준다. 그리고 같은 extension 블럭 안에 있는 `cellForItemAt` 함수에 호출할  
setCell 함수를 만들어 주기 위해,, collectionView cell swift 파일로 가서 setCell 함수를 만들어준다.

**smallCollectionViewCell.swift**

```swift
    func setCell(project: ProjectModel) {
        self.smallCellImageView.image = UIImage(named: project.image)
        self.smallSubHeadLabel.text = "\(project.category) | \(project.company)"
        self.smallHeadlineLabel.text = project.title
        self.smallSubHead2Label.text = "\(project.percentage) 달성"
    }
```

그리고 다시 tableviewcell swift 파일로 와서, set cell 함수를 호출해주면 된다.  
또 바깥으로 한 칸 나와서 main VC swift 파일에서도 set cell 함수 호출 !!

![image](https://user-images.githubusercontent.com/28949235/118607155-38094680-b7f3-11eb-8ef3-1a41bbf29af2.png)



정리하자면 !!!!!!

