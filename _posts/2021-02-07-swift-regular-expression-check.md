---
layout: post
title: "swift 정규식 검사" 
date: 2021-02-07
category: read 
excerpt: ""

---

# swift 정규식 검사

```swift
// 비밀번호 정규식 검사
func validatePassword(string: String) -> Bool {
    let passwordRegEx = "^[a-zA-Z0-9!@#$%^&*(),.?\":{}|<>_-]{6,}$"
        
    let predicate = NSPredicate(format:"SELF MATCHES %@", passwordRegEx)
    return predicate.evaluate(with: string)
}
```

