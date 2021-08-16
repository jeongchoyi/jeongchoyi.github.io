---
layout: post
title: "JTAppleCalendar 캘린더 커스텀하기-1" 
date: 2021-08-10
category: read 
excerpt: ""


---

# JTAppleCalendar 캘린더 커스텀하기-1

세부적인 뷰 나오기 전에 연습용 ~.~  
대충 어떻게 돌아가는지만 미리 확인 해 보려고 한다.

[JTAppleCalendar repo](https://github.com/patchthecode/JTAppleCalendar)

<img src="https://user-images.githubusercontent.com/28949235/128822394-0ec67a7c-0925-4e32-90fb-2be2f709dfd1.png" alt="image" width=300px /> <img src="https://user-images.githubusercontent.com/28949235/128822473-da05b64b-937f-49be-8968-c9920f9eb26b.png" alt="image" width=300px />

collectionview autolayout 잡아주고, Class 랑 Module도 잡아준다.

<img src="https://user-images.githubusercontent.com/28949235/128822605-7f9838c8-0a0b-45a4-89d2-07af1a7760ce.png" alt="image" width=300px />

Minimum Spacing 기본값 10, 10을 0,0으로 바꿔준다.

그 다음에 cell을 디자인 해 주면 되는데, 사용자가 원하는 cell을 마음대로 생성하면 된다고 한다 !!  
대신 중요한 건 **cell의 identifier를 `dateCell` 로 설정해줘야 한다는 것!**  
예제에서는 따로 xib 파일을 만들지 않고 default로 생성되는 cell에 작업했다.

```swift
import JTAppleCalendar

class DateCell: JTACMonthCell {
    
}

```

그리고 cell Class를 만들어준다. 튜토리얼에서는 `JTAppleCell` 이라고 나와있는데,

**'JTAppleCell' is unavailable: 'JTAppleCell' has been renamed to '_TtC15JTAppleCalendar11JTACDayCell'**

이런 오류가 떴고, [업데이트 내역](https://patchthecode.com/jtapplecalendar-version-8-0-0-migration-guide/)을 살펴보니 JTAppleDayCell이 됐다가, 최종적으로  JTACMonthCell이 된 것 같다.  
( 변경 이유는 YearView를 제공하기 위함이라고 ..!!)

그 다음으로는 JTACMonthViewDelegate, JTACMonthViewDataSource를 만들어준다.

![image](https://user-images.githubusercontent.com/28949235/128825550-af33b91e-b4a9-4930-9496-14faacb76335.png)

근데 여기서 반환값이 JTACDayCell이여서, 위에서 만든 Cell Class를 JTACDayCell로 바꿔줬다...  
DataSource는 아래처럼 작성해주고,

```swift
extension ViewController: JTACMonthViewDataSource {
    func configureCalendar(_ calendar: JTACMonthView) -> ConfigurationParameters {
        let formatter = DateFormatter()
        formatter.dateFormat = "yyyy MM dd"
        let startDate = formatter.date(from: "2021 08 01")!
        let endDate = Date()
        return ConfigurationParameters(startDate: startDate, endDate: endDate)
    }
}
```

Delegate는 아래처럼 작성해준다.

```swift
extension ViewController: JTACMonthViewDelegate {
    func calendar(_ calendar: JTACMonthView, willDisplay cell: JTACDayCell, forItemAt date: Date, cellState: CellState, indexPath: IndexPath) {
        let cell = cell as! DateCell
        cell.dateLabel.text = cellState.text
    }
    
    func calendar(_ calendar: JTACMonthView, cellForItemAt date: Date, cellState: CellState, indexPath: IndexPath) -> JTACDayCell {
        let cell = calendar.dequeueReusableJTAppleCell(withReuseIdentifier: "dateCell", for: indexPath) as! DateCell
        cell.dateLabel.text = cellState.text
        return cell
    }
}
```

권한 위임 해주고, 빌드!

```swift
calendarCollectionView.ibCalendarDelegate = self
calendarCollectionView.ibCalendarDataSource = self
```

<img src="https://user-images.githubusercontent.com/28949235/128826700-eac392f4-8076-411f-b9f1-d02e28713637.png" alt="image" width=300 />

짜잔
