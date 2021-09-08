---
layout: post
title: "UICollectionView MVVM 예제로 살펴보기" 
date: 2021-09-08
category: read 
excerpt: ""
---

# UICollectionView MVVM 예제로 살펴보기

<img src="https://user-images.githubusercontent.com/28949235/132507397-68aedbc1-3a44-4e27-8a9c-6071788463ed.png" alt="image" style="zoom:50%;" />

요 뷰를 앱잼 기간 때 MVVM으로 마이그레이션 해 놨었는데,  
따로 정리를 해 놓지 않아서.. 이번에 정리하면서 다시 정확하게 이해하려고 한다!
(MVVM 처음 써봐서 코드가 완전 100% 정답인지는 모르겠음)

달랑 CollectionView 하나 있는 간단한 뷰 컨트롤러인데,
클릭 이벤트 등은 제외하고, 딱! 데이터를 서버에서 받아와서 fetch하고, 표시하는 것만 다뤄보자,,

### ViewModel.swift

```swift
class CourseListViewModel {
    
     var courses = [Course]()
    
    func getCourseLibrary(completion: @escaping ((ViewModelState) -> Void)) {
        CourseAPI.shared.getCourseLibrary { (response) in
            switch response {
            case .success(let courses):
                if let data = courses as? CoursesData {
                    self.courses = data.courses
                    completion(.success)
                }
            case .requestErr(let message):
                print("requestErr", message)
                completion(.failure)
            case .pathErr:
                print(".pathErr")
                completion(.failure)
            case .serverErr:
                print("serverErr")
                completion(.failure)
            case .networkFail:
                print("networkFail")
                completion(.failure)
            }
        }
    }
}
```

요로코롬... CourseListViewModel 클래스에는
받아온 데이터를 저장할 변수와, 서버에서 통신한 결과를 까서 데이터를 원하는 변수에 저장해주는 함수가 있다.

```swift
struct CourseViewModel {
    let course: Course
    
    // 의존성 주입 (Dependency Injection)
    init(_ course: Course) {
        self.course = course
    }
}
```

그리고 CourseViewModel 구조체에는
1개의 Course Model 데이터를 받아 저장하는 상수를 만들어 주고,  
의존성 주입(DI)을 위한 init 함수를 작성해준다.

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

그리고 CourseListViewModel의 extension을 작성하는데,  
여기서는 View에서 사용하지만 Data에 관련이 있는 내용들 (?)을 작성해준다.
CollectionView에서는 numberOfSections, numberOfRowsInSection, 특정 idx의 데이터값 등이 있겠다
이 때, 특정 idx의 데이터값은 CourseViewModel 형을 반환한다.

즉, CourseListViewModel은 말 그대로 Course의 List 관련,
CourseViewModel은 단일 Course 관련이다!

이런식으로 작성해주고, 뷰 컨트롤러에서 직접 사용해보자

### ViewController.swift

```swift
    private func fetchCourses() {
        courseListViewModel.getCourseLibrary { state in
            switch state {
            case .success:
                return self.updateUI()
            case .failure:
                return
            }
        }
    }
    
    private func updateUI() {
        self.courseLibraryCollectionView.reloadData()
    }
```

서버에서 데이터를 fetch하는 fetchCourses() 함수와,  
그 과정이 성공했을 때 UI를 업데이트 하는 함수를 만들어준다.

지금은 그냥 reload 한 줄이지만, 나중을 대비해 함수로 빼 놨다.

#### UICollectionViewDataSource

```swift
// MARK: - UICollectionViewDataSource

extension CourseLibraryViewController: UICollectionViewDataSource {
    
    func numberOfSections(in collectionView: UICollectionView) -> Int {
        return courseListViewModel.numberOfSections
    }
    
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return courseListViewModel.numberOfRowsInSection(0)
    }
```

numberOfSections, numberOfItemsInSection은 우리가 viewmodel의 extension에서 작성한
내용들을 그대로 써 주면 된다.

개인적으로는 이 부분에서 View와 Data(?)가 분리되어 있는 느낌을 받았다.
Data에 관련된 건 ViewModel이 전담하는 느낌이라... 마음이 편안해지는 느낌...( ◠‿◠ ) ..

```swift
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        
        switch courseListViewModel.courseAtIndex(indexPath.row).course.situation {
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

cellForItemAt 함수를 보면, 지금 현재 내가 개발하는 뷰에서는
course의 situation에 따라 다른 CollectionViewCell를 반환해야 한다.

```swift
switch courseListViewModel.courseAtIndex(indexPath.row).course.situation {
```

CourseListViewModel의 courseAtIndex를 써서 courseData를 받아온 후, course의 situation에 따라 분기 해 준다.

```swift
if let cell = collectionView.dequeueReusableCell(withReuseIdentifier: Const.Xib.Identifier.undoneCourseCollectionViewCell, for: indexPath) as? UndoneCourseCollectionViewCell {
                
let viewModel = courseListViewModel.courseAtIndex(indexPath.row)
cell.courseViewModel = viewModel
                
return cell
}
```

cell을 정의한 후 cell에 데이터를 넘겨 줄 때에도, ViewModel로 해결한다.  
`courseListViewModel.courseAtIndex(indexPath.row)`, 
(CourseViewModel 형)을 viewModel이라는 상수에 할당해주고,

cell의 courseViewModel이라는 변수에 그 값을 넘겨준다.

### cell.swift

cell의 courseViewModel에 우리가 해당 idx에 해당하는 course view model을 넘겨줬는데,  
cell 파일을 보면 아래와 같이 작성되어있다.

```swift
class UndoneCourseCollectionViewCell: UICollectionViewCell {
    
    // MARK: - Properties
    
    var courseViewModel: CourseViewModel! {
        didSet {
            titleLabel.text = courseViewModel.course.title
            courseDaysLabel.text = "\(courseViewModel.course.totalDays)일"
            descriptionTextView.text = courseViewModel.course.courseDescription
            setProperty(by: courseViewModel.course.property)
        }
    }
```

courseViewModel 변수는 우리가 넘겨준 CourseViewModel 형이고,  
값이 들어오면 그 값이 didSet을 통해 뷰의 여러 오브젝트들에 반영되거나, 매개변수로 전달된다.
