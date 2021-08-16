---
layout: post
title: "UserDefaults fcm token" 
date: 2021-07-13
category: read 
excerpt: ""

---

# UserDefaults - fcm token

처음 로그인 할 때 fcm token을 넘겨줘야 하는데,  
fcm token은 앱을 launch할 때 생성된다.

그래서 UserDefaults에 넣어놓고 로그인 때 사용할 수 있도록 하려고 한다 ~~    
한번만 보내면 되는거라 꼭 UserDefaults를 써야하는 건 아닌데,  
나중에 jwt토큰은 무조건 UserDefaults에 넣어놔야 해서, 팀원들에게 사용법 알려주려고 미리 쓰는 글

**UserDefaults는 [데이터, 키(key)]으로 데이터를 저장한다!**

> 참고: UserDefaults는 앱 지우면 같이 지워짐

### 값 저장하기

```swift
UserDefaults.standard.set(true, forKey: "키 이름")
```

```swift
UserDefaults.standard.setValue(signInData.token, forKey: "token")
```

### 값 사용하기

```swift
UserDefaults.standard.string(forKey: "token")
```

```swift
UserDefaults.standard.bool(forKey: "didLaunch")
```

```swift
UserDefaults.standard.integer(forKey: "userId")
```

```swift
UserDefaults.standard.value(forKey: "stringData") as! String
```

### 값 지우기

```swift
UserDefaults.standard.removeObject(forKey: "token")
```

## Journey app에서의 UserDefaults

### fcm Token

```swift
UserDefaults.standard.set(fcmTokenString, forKey: "fcmToken")
```

```swift
 "userToken": UserDefaults.standard.string(forKey: "fcmToken")
```

### jwt Token

```swift
UserDefaults.standard.set(data.jwt, forKey: "jwtToken")
```

```swift
UserDefaults.standard.string(forKey: "jwtToken")
```

### 로그인 여부 판단하기

```swift
// 로그인 성공 시
UserDefaults.standard.set(data.jwt, forKey: "jwtToken")
```

```swift
// sceneDelegate.swift
private func hasJwtToken() -> Bool {
    return UserDefaults.standard.object(forKey: "jwtToken") != nil
}

// willConnectTo
if !hasJwtToken() {
    setRootViewControllerToLogin()
    } else {
    setRootViewControllerToTabbar()
}
```

