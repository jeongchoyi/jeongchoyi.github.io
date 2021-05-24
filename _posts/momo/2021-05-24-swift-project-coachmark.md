---
layout: post
title: "코치마크 추가하기" 
date: 2021-05-24
category: read 
excerpt: ""

---

# 코치마크 추가하기

MOMO 업데이트 ! 코치마크 추가하기  
**로그인 이벤트가 발생할 때 마다** 코치마크를 보여준다.  
코치마크 뷰 어디를 클릭하든 다음 코치마크 또는 다음 플로우로 넘어간다.

![image](https://user-images.githubusercontent.com/28949235/119291273-23143380-bc89-11eb-8c45-a4ab187d5a40.png)

코치마크는 이렇게 생겼다. 메인화면 위에 반투명한 뷰가 2개 !!  
로그인 후 처음 메인에 진입할 때, 두 코치마크 뷰를 보여주면 될 것 같다.  
반투명 view니까... Xib로 만들어서 메인 뷰 앞에 붙이는걸로 ... ?

### Xib 만들기

> CoachmarkFirstView.swift

![image](https://user-images.githubusercontent.com/28949235/119291593-adf52e00-bc89-11eb-9714-3a7338ab67f5.png)

배경색을 Opacity 있게 지정해준다.

![image](https://user-images.githubusercontent.com/28949235/119296087-45ab4a00-bc93-11eb-8e59-454c00b12548.png)

그리고 컴포넌트들을 배치해준다.

### Xib 메인 뷰의 UIView로 연결하기

메인 뷰의 스토리보드에 UIView를 맨 위에 놓아주고, @IBOutlet 연결도 해준다.

```swift
// 코치마크 뷰 register
coachmarkView.frame = CGRect(x: 0, y: 0, width: UIScreen.main.bounds.width, height: UIScreen.main.bounds.height)
if let customView = Bundle.main.loadNibNamed(Constants.Name.coachmarkFirstViewXib, owner: nil, options: nil)?.first as? UIView {
customView.frame = self.coachmarkView.bounds
coachmarkView.addSubview(customView)
}
```

요렇게 해주면 연결 완료 !

### 로그인 할 때마다 코치마크가 보이게 하기

앱을 처음 실행 시에만 코치마크가 보이게 하는 거면  
온보딩 분기처리에 쓰는 `UserDefaults.standard.bool(forKey: "didLaunch")`를 사용하면 되지만,  

"앱을 처음 실행 시"가 아니라 "로그인을 했을 때"마다 보여줘야 하는 거라  
새로운 UserDefaults 변수를 만들어줘야 한다.

* default : false (로그인 X)
* 로그인, 회원가입 시 : true로 변경 (로그인 O)
* 로그아웃, 탈퇴 시 : false로 변경 (로그인 X)

`UserDefaults.standard.set(false, forKey: "didLogin")`

근데 로그인, 회원가입 했다고 바로 true로 바꿔버리면 메인VC를 열 때마다 true일거고,  
정작 코치마크를 보여줄 분기처리를 하지 못하기 때문에  
**코치마크를 닫을 때 true로** 바꿔줘야 한다. (그럼 이름을 "didLogin"으로 하는게 별로일 것 같긴 하다..)

혹시 모르니까 로그인, 회원가입할 땐  
`UserDefaults.standard.setValue(false, forKey: "didLogin")`로 false값을 지정해준다.  
확실히 하기 위해서...

- [x] 회원가입, 이메일 로그인, 애플 로그인, 카카오 로그인 시 "didLogin"값 false 지정
- [x] 코치마크를 닫을 때 true 지정
- [ ] 로그아웃, 탈퇴 시 false 지정 -> 이건.. 안 해도 되네.. (1번 때문에)  
  (어차피 앱 사용 시 로그인은 필수니까) 

### 코치마크 진행하기 (UIView에 터치이벤트 주기)  

코치마크의 개수는 총 2개고,  
코치마크1 --> 코치마크2 --> 메인 뷰  
이기 때문에 **코치마크를 2번 터치했을 땐 코치마크 뷰가 hidden 처리** 되어야 한다.

```swift
var coachmarkTouchCount = 0
```

터치 카운트 변수를 만들어 주고, UIView에 tap gesture recognizer를 붙인다.

```swift
@IBOutlet weak var coachmarkView: UIView!
```

UIView도 @IBOutlet 연결 되어있는 상태 !



**UITapGestureRecognizer 붙이기**

```swift
let coachmarkGesture: UITapGestureRecognizer = UITapGestureRecognizer(target: self, action: #selector(핸들러함수이름(_:)))
coachmarkView.addGestureRecognizer(coachmarkGesture)
```

```swift
@objc func 핸들러함수이름(_ gesture: UITapGestureRecognizer) {
  	// 동작
}
```

요런 식으로 붙여주면 된다.

![image](https://user-images.githubusercontent.com/28949235/119339510-3399cd80-bccc-11eb-8369-f526e43378bb.png)

어우.. 안보이지만 요런 느낌으로 count에 따라 뷰를 바꿔주고, isHidden 시키면 된다.  
(이후 조금 많이 수정했지만 굵직한 로직은 저런 느낌..!!!)

![image](https://user-images.githubusercontent.com/28949235/119339610-54fab980-bccc-11eb-869d-e57b159db230.png)

코치마크가 끝나면 "didLogin" UserDefaults 값을 true로 바꿔주는 것도 잊지 말기 !

