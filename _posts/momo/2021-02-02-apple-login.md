---
layout: post
title: "애플 로그인 구현하기" 
date: 2021-02-02
category: read 
excerpt: ""

---

# 애플 로그인 구현하기

앱에서 소셜 로그인을 하나라도 쓴다면 Apple 로그인은 필수로 지원해야 한다. [앱 스토어 리뷰 가이드라인](https://developer.apple.com/app-store/review/guidelines/#sign-in-with-apple)

![image](https://user-images.githubusercontent.com/28949235/106629906-a9afe280-65be-11eb-8964-42706c75df7e.png)

`Signing & Capabilities` 에서 `Capability` 왼쪽의 `+` 버튼을 누른다.



![image](https://user-images.githubusercontent.com/28949235/106629996-c21ffd00-65be-11eb-9d2c-7067661ea3c4.png)

`Sign in with Apple` 을 더블클릭해 추가해준다.

![image](https://user-images.githubusercontent.com/28949235/106630035-cba96500-65be-11eb-9883-5b7c11f733b3.png)

요게 추가된 모습!

![image](https://user-images.githubusercontent.com/28949235/106630645-5b4f1380-65bf-11eb-82aa-7bed27951669.png)

 에서는 StackView를 만들고 거기다  
`ASAuthorizationAppleIDButton()` 생성자를 사용해 버튼을 만든 후,  
`addArrangedSubview`를 해줘서 버튼을 붙이는 식으로 하길래,  
우선은 똑같이 따라 해 보기로 했다. 이를 위한 StackView를 만드는 모습,,



```swift
import AuthenticationServices
```

요 놈을 먼저 import 해준다.



```swift
// MARK: - Functions
func setupProviderLoginView() {
    let authorizationButton = ASAuthorizationAppleIDButton()
    authorizationButton.addTarget(self, action: #selector(handleAuthorizationAppleIDButtonPress), for: .touchUpInside)
    self.loginProviderStackView.addArrangedSubview(authorizationButton)
}
```

위와 같은 함수를 만들어준다. 그리고 `viewDidLoad()`에서 `setupProviderLoginView()` 를 호출해준다.

> 꼭 `ASAuthorizationAppleIDButton()`으로 버튼을 만들어야 하는 줄 알았는데, 아니였다 ( ◠ ◡ ◠ )



추가로, `authorizationButton.cornerRadius = 33` 와 같이 radius 값이나,   
style 등을 지정해줄 수 있다. [공식 문서](https://developer.apple.com/documentation/authenticationservices/asauthorizationappleidbutton) 참고!



```swift
@objc func handleAuthorizationAppleIDButtonPress(){
        
}
```

그리고 버튼이 클릭되었을 때 호출 될 함수를 만들어준다.

> 그냥 내가 만든 버튼 @IBAction 함수 써도 됨 ( ◠ ◡ ◠ )



```swift
@objc func handleAuthorizationAppleIDButtonPress(){
    let appleIDProvider = ASAuthorizationAppleIDProvider()
    let request = appleIDProvider.createRequest()
    request.requestedScopes = [.fullName, .email]
        
    let authorizationController = ASAuthorizationController(authorizationRequests: [request])
    authorizationController.delegate = self
    authorizationController.presentationContextProvider = self
    authorizationController.performRequests()
}
```

내용은 위와 같이 채워준다.  
delegate, presentationContextProvider도 extension으로 구현해준다.



```swift
extension LoginViewController: ASAuthorizationControllerDelegate {
    func authorizationController(controller: ASAuthorizationController, didCompleteWithAuthorization authorization: ASAuthorization) {
        switch authorization.credential {
        case let appleIDCredential as ASAuthorizationAppleIDCredential:
            
            // Create an account in your system.
            let userIdentifier = appleIDCredential.user
            let fullName = appleIDCredential.fullName
            let email = appleIDCredential.email
            
            // 서버에 넘겨줘야 하는 identityToken 값
            if let identityToken = appleIDCredential.identityToken,
          		 // identityToken은 Data? 라서, 아래와 같이 인코딩해줘야 함
               let tokenString = String(data: identityToken, encoding: .utf8) {
                print(tokenString)
            }
            
            // View Transition
            self.pushToHomeViewController()
        
        case let passwordCredential as ASPasswordCredential:
        
            // Sign in using an existing iCloud Keychain credential.
            let username = passwordCredential.user
            let password = passwordCredential.password
            
            // For the purpose of this demo app, show the password credential as an alert.
            DispatchQueue.main.async {
                // self.showPasswordCredentialAlert(username: username, password: password)
            }
            
        default:
            break
        }
    }
    func authorizationController(controller: ASAuthorizationController, didCompleteWithError error: Error) {
        print(error)
    }
    
}

extension LoginViewController: ASAuthorizationControllerPresentationContextProviding {
    func presentationAnchor(for controller: ASAuthorizationController) -> ASPresentationAnchor {
      	// 사용자에게 로그인 요청 창을 띄울 window 지정
        return view.window!
    }
}
```



난 디자인 때문에 내가 만든 버튼에 적용했는데, 이게 심사가 통과되는지는 모르겠다.  
저걸 강제하는건가?.. 리젝당하는 거 아녀,,? ㅎ



아 그리고! 시뮬레이터로는 오류 뱉고 무한 로딩이 뜰 수 있는데,  
기기 연결해서 빌드하면 그런 현상이 사라진다!

### 추가

리젝 당했음 ㅋㅋ

꼭.. 애플 아이콘을 쓰세요..