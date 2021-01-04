---
layout: post
title: "UITableView Custom Section Header" 
date: 2021-01-01
category: read 
excerpt: ""

---

# UITableView Custom Section Header

### 스토리보드 내 설정

![image](https://user-images.githubusercontent.com/28949235/103515938-f9fa2e80-4eb2-11eb-9c27-cb5eefd02351.png)

스토리 보드 설정 - Grouped  
이렇게 해야 header가 section 중간에서도 sticky하지 않는다,,  
설명을 못하겠네  
section header가 스크롤했을때 위에서 floating되지 않고 같이 스크롤되어 올라간다! !



### section 높이 지정

그리고 아래와 같은 함수를 구현해줘야 한다.

```swift
func tableView(_ tableView: UITableView,
                   heightForHeaderInSection section: Int) -> CGFloat {
    return CGFloat(70.0)
}
```

> 10년도 더 된 버그인데,,  
> 요 함수를 구현해주지 않으면 첫번째 헤더(index 0인거)가 안 나오는 버그가 있다고 한다.

### section 하단 여백 삭제

![image](https://user-images.githubusercontent.com/28949235/103517265-33cc3480-4eb5-11eb-88a3-d176f6f115b5.png)

저기 하얀색만큼 여백(footer)이 생기는데, 고걸 지워주는 법

```swift
func tableView(_ tableView: UITableView, heightForFooterInSection section: Int) -> CGFloat {
    return 0 
}
```

0으로 주면 지워질 줄 알았는데 아니였다.

```swift
func tableView(_ tableView: UITableView, heightForFooterInSection section: Int) -> CGFloat {
    return CGFloat.leastNormalMagnitude
}
```

CGFloat의 min값으로 줘야 사라진다.

### section header 뷰

```swift
func tableView(_ tableView: UITableView, viewForHeaderInSection section: Int) -> UIView? {
	let view = UIView(frame: CGRect(x: 0, y: 0, width: tableView.frame.size.width, height: 18))
	let label = UILabel(frame: CGRect(x: 10, y: 5, width: tableView.frame.size.width, height: 18))
	label.font = UIFont.systemFont(ofSize: 14)
	label.text = "\(section)" 
	view.addSubview(label)
	view.backgroundColor = UIColor.gray 
	return view
}
```

이런 식으로 뷰 짜주면 됨,, 난 걍 대충 숫자만 써놨다

