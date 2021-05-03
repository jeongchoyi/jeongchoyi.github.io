---
layout: post
title: "swift keyboard observer" 
date: 2021-03-01
category: read 
excerpt: ""

---

# swift keyboard observer

```swift

    
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(true)
        
        initializeKeyboardObserver()
    }
    
    override func viewWillDisappear(_ animated: Bool) {
        super.viewWillDisappear(true)
        
        removeKeyboardObserver()
    }
```

```swift
    func initializeKeyboardObserver() {
        // keyboardWillShow, keyboardWillHide observer 등록
        NotificationCenter.default.addObserver(self, selector: #selector(keyboardWillShow(_:)), name: UIResponder.keyboardWillShowNotification, object: nil)
        NotificationCenter.default.addObserver(self, selector: #selector(keyboardWillHide(_:)), name: UIResponder.keyboardWillHideNotification, object: nil)
    }
    
    func removeKeyboardObserver() {
        // observer 제거
        NotificationCenter.default.removeObserver(self, name: UIResponder.keyboardWillShowNotification, object: nil)
        NotificationCenter.default.removeObserver(self, name: UIResponder.keyboardWillHideNotification, object: nil)
    }
```

```swift
    
    
    @objc func keyboardWillShow(_ notification: NSNotification) {
        if let keyboardFrame: NSValue = notification.userInfo?[UIResponder.keyboardFrameEndUserInfoKey] as? NSValue {
            let keyboardRectangle = keyboardFrame.cgRectValue
            let keyboardHeight = keyboardRectangle.height
            
            getPasswordButtonBottom.constant = CGFloat(getPasswordButtonBottomConstraint) + keyboardHeight
        }
    }
    
    @objc func keyboardWillHide(_ notification: NSNotification) {
        if let keyboardFrame: NSValue = notification.userInfo?[UIResponder.keyboardFrameEndUserInfoKey] as? NSValue {
            let keyboardRectangle = keyboardFrame.cgRectValue
            let keyboardHeight = keyboardRectangle.height
            
            getPasswordButtonBottom.constant = CGFloat(getPasswordButtonBottomConstraint)
        }
    }
```

```swift
    extension FindPasswordViewController: UITextFieldDelegate {
        func textFieldDidEndEditing(_ textField: UITextField) {
            
            // 이메일
            if textField == emailTextField {
                checkEmail()
                
                // 비밀번호
            }
        }
    }

```

