---
layout: post
title: "키보드 내리기(화면 터치, return 버튼 클릭 시)" 
date: 2021-06-19
category: read 
excerpt: ""

---

## 키보드 내리기(화면 터치, return 버튼 클릭 시)

### 화면 아무 곳이나 터치했을 때

```swift
override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
    self.view.endEditing(true)
}

```

### return 버튼 클릭 시

```swift
extension NicknameViewController: UITextFieldDelegate {
    func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        self.view.endEditing(true)
        return true
    }
}
```

( delegate 위임 해주고 ~~)
