---
layout: post
title: "How to Migrate MVC to MVVM - 2" 
date: 2021-07-02
category: read 
excerpt: ""

---

# iOS 프로젝트에 MVVM 도입하기 - 2

> 기존 코드 MVVVM으로 변경해보기

<img src="https://user-images.githubusercontent.com/28949235/124230964-a8251f00-db4a-11eb-8311-395855a54a7d.gif" alt="Screen-Recording-2021-07-02-at-3 30 12-PM" width=200px />

MVC로 미리 짜 놨던 요 뷰를 MVVM으로 migration 해보려고 한다.

### Model

모델은 짜 놓은 것 그대로 쓴다.  (나중에 서버 통신할 땐 Decodable 등,, 추가 하겠지만)

<img src="https://user-images.githubusercontent.com/28949235/124231367-208be000-db4b-11eb-8069-8f031c485f17.png" alt="image" width=300px />

서비스 함수는 API가 안 나왔으니까 넘겨두고, 더미데이터로 진행한다

### View Model

ViewModel에서는 Collectionview Delegate, DataSource 함수까지 다 정의 해줄 것!

<img src="https://user-images.githubusercontent.com/28949235/124231615-76608800-db4b-11eb-9752-daa4224e894c.png" alt="image" width=400px />

뷰 모델 swift 파일을 만들어준다.  
이제 내용을 작성할건데, 먼저 기존 MVC 패턴에서의 controller의 구성은 아래와 같다. 

<img src="https://user-images.githubusercontent.com/28949235/124231935-ea9b2b80-db4b-11eb-9629-de53a9222826.png" alt="image" width=400px />

MVVM에서 중요한 포인트는 View에는 UI관련만 !! 있어야 하고,  
그 외 로직들은 VM에 있어야 한다.

그래서  
여기에서 `initNavigationBar()`, `registerXib()`, `assignDelegation()` 같은 경우는 View에 그대로 남겨놓는다.  
그 외에 UICollectionViewDataSource 내부에서는  
ViewModel 내의 함수를 호출만 ! 한다.  

이제 ViewModel을 작성해보자 ~!~!

> Class로 만들수도 있는 것 같은데, 우선 struct로 진행..!!

```swift
struct CoursesViewModel {
    
}
```

요렇게 만들어준다. 

### 의존성 주입 (DI)

이제 의존성 주입을 위한 init함수를 만들어준다.

<img src="https://user-images.githubusercontent.com/28949235/124231367-208be000-db4b-11eb-8069-8f031c485f17.png" alt="image" width=300px />

모델 참고하면서 작성!

```swift
struct CoursesViewModel {
  
  	let title: String
  	let description: String
  	let courseDays: Int
  	let situation: Int
  	let property: String
    
  	// 의존성 주입 (Dependency Injection)
  	init(course: Course) {
      self.title = course.title
      self.description = course.description
      self.courseDays = course.courseDays
      self.situation = course.situation
      self.property = course.property
    }
}
```

이렇게 해도 되고,  요 부분을 더 깔끔하게 하고 싶으면  
Model에서 init 함수를 만들어준 후

```swift
struct CoursesViewModel {
  
		let course: Course
  
  	init(course: Course) {
      self.course = course
    }
}
```

요렇게 해줘도 된다. 깔끔 ~!!

> Model에서 init함수?
>
> ```swift
> struct Course {
>     let id: Int
>     let title: String
>     let description: String
>     let courseDays: Int
>     let situation: Int
>     let property: String
>   
>   	init(id: Int, title: String, description: String, courseDays: Int, situation: Int, property: String) {
>       self.id = id
>       self.title = title
>       self.description = description
>       self.courseDays = courseDays
>       self.situation = situation
>       self.property = property
>     }
> }
> ```
>
> 요렇게 만들어주면 된다.

> 사실 뭐... 방법은 엄청 많다.
>
> ```swift
> extension CoursesViewModel {
>     var title: String {
>         return self.course.title
>     }
>     
>     var description: String {
>         return self.course.description
>     }
>     
>     var courseDays: Int {
>         return self.course.courseDays
>     }
>     
>     var situation: Int {
>         return self.course.situation
>     }
>     
>     var property: String {
>         return self.course.property
>     }
> }
> ```
>
> 이런 식으로 해 줘도 되고...

### VC에서 View Model 사용하기

> VC.swift 파일에서

```swift
var viewModel: CoursesViewModel!
```

이렇게 viewModel 변수를 만들어주고, 데이터를 넣어주면 된다.

```swift
// course data가 있다고 가정
viewModel = CoursesViewModel(course: course)
```

데이터에 따라 초기에 label의 text나 뭐 background color 등을 지정해주고 싶으면, didSet을 사용하면 된다.

```swift
var viewModel: CoursesViewModel! {
  didSet {
    //여기에 작성
    self.someLabel.text = viewModel.course.title
    navigationItem.title = viewModel.course.title
  }
}
```

뭐 이런 식으로다가..

### 진짜 본격적으로 View Model 작성하기

이제 진짜 뷰 모델 작성을 해볼 건데 !!

```swift
struct CoursesViewModel {
  
		let course: Course
  
  	init(course: Course) {
      self.course = course
    }
}
```

아까 짠 뷰 모델로는 cell만 표현 가능하고, 뷰 전체에 대한 data를 표현할 수 없다. (course는 한 cell의 데이터니까!)  
그래서 아래와 같이 만들어준다.

```swift
struct CourseListViewModel {
    let courses: [Course]
}

struct CourseViewModel {
    let course: Course
    
    // 의존성 주입 (Dependency Injection)
    init(_ course: Course) {
        self.course = course
    }
}
```

그리고 tableview delegate, dataSource에 넘겨줄 것들을 작성해준다.

```swift
extension CourseListViewModel {
    var numberOfSections: Int {
        return 1
    }
    
    func numberOfRowsInSection(_ section: Int) -> Int {
        return self.courses.count
    }
    
    func courseAtIndex(_ index: Int) -> CourseViewModel {
        let course = self.courses[index]
        return CourseViewModel(course)
    }
}
```



### VC에서

```swift
// MARK: - UICollectionViewDataSource

    func numberOfSections(in collectionView: UICollectionView) -> Int {
        return courseListViewModel.numberOfSections
    }
    
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return courseListViewModel.numberOfRowsInSection(0)
    }
    
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        
        switch courses[indexPath.row].situation {
        case 0:
            if let cell = collectionView.dequeueReusableCell(withReuseIdentifier: Const.Xib.Identifier.undoneCourseCollectionViewCell, for: indexPath) as? UndoneCourseCollectionViewCell {
                
                let viewModel = courseListViewModel.courseAtIndex(indexPath.row)
                cell.courseViewModel = viewModel
                cell.setButtonTitle(doingCourse: doingCourse)
                
                return cell
            }
            
        case 2:
            if let cell = collectionView.dequeueReusableCell(withReuseIdentifier: Const.Xib.Identifier.doneCourseCollectionViewCell, for: indexPath) as? DoneCourseCollectionViewCell {
                
                let viewModel = courseListViewModel.courseAtIndex(indexPath.row)
                cell.courseViewModel = viewModel
                cell.setButtonTitle(doingCourse: doingCourse)
                
                return cell
            }
        default:
            return UICollectionViewCell()
        }
    
        return UICollectionViewCell()
    }
```

이런 식으로 사용해주면 된다.



### 명심 포인트

VC에서는 UI 관련 task만 수행~!!!!!
