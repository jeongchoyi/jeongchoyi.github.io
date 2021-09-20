---
layout: post
title: "UIImagePickerController" 
date: 2021-09-20
category: read 
excerpt: ""

---

# UIImagePickerController

<img src="https://user-images.githubusercontent.com/28949235/133974026-449700cb-448d-4a06-90ba-2cd777c4c9bd.png" alt="simulator_screenshot_A028D85A-BDC6-4A12-A4FC-C26A40CE2A35" width=300 />

POPO 표지 선택 타입 중 하나, 사용자가 12개의 사진을 선택해서 올리는 뷰다.  
사용자의 갤러리에 있는 사진을 업로드하고, 서버로 보내기 전까지 개발할 예정!

### UIImagePickerController 만들기

```swift
var photoPicker = UIImagePickerController()
```

UIImagePickerController를 선언해준다.

### 권한 위임하기

```swift
private func assignDelegate() {
    imagePicker.delegate = self
}
```

delegate를 위임해준다.

### extension 만들기

![image](https://user-images.githubusercontent.com/28949235/133974617-4fff6fef-6638-457b-9fe5-1a11dc94bbef.png)

요런 오류가 뜨는걸로 봐서,  
UIImagePickerControllerDelegate, UINavigationControllerDelegate 둘을 상속받는  
extensions을 만들어줘야 하는 것 같다.

> 왜 UINavigationControllerDelegate도 상속받아야 하는지 궁금해서 찾아봤더니,
>
> UIImagePickerControllerDelegate의 delegate 속성은 UIImagePickerControllerDelegate와 UINavigationControllerDelegate 프로토콜을 모두 구현하는 객체로 정의되어있다. 
>
> (위에서 해준 picker.delegate =  self) self를  picker.delegate에 할당하려면 self는 UINavigationControllerDelegate 타입이어야 한다. 
>
> 지금, picker의 델리게이트를 UINavigationControllerDelegate에 위임해준 것인데, 대리자는 사용자가 이미지나 동영상을 선택하거나 picker화면을 종료할 때, 알림을 받는다. 
>
> 라고 한다. [출처](https://zeddios.tistory.com/125)

```swift
extension CoverUserPhotoViewController: UIImagePickerControllerDelegate, UINavigationControllerDelegate  {
    
    // didFinishPickingMediaWithInfo
    func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
       	
    }
}
```

`didFinishPickingMediaWithInfo` 함수도 선언해준다.  
[docs](https://developer.apple.com/documentation/uikit/uiimagepickercontrollerdelegate/1619126-imagepickercontroller) 를 보면, 유저가 사진이나 동영상을 선택했음을 delegate에게 알려준다는 함수다.
이따 여기서 서버에 보낼 데이터에 이미지도 저장하고, 표지 이미지뷰의 이미지도 바꿔줄 것이다.

### photoLibrary 띄우기

이제 사용자가 각 `+` 뷰를 눌렀을 때, 각각의 표지 뷰에 들어갈 사진을 고를  
photoLibrary(갤러리)가 띄워져야 한다.

각각의 `+` 이미지 뷰에 `UITapGestureRecognizer`를 달아준다. 

```swift
    private func initTapGesterRecognizer() {
        for (index, coverImageView) in coverImageViews.enumerated() {
            let coverTapRecognizer = UITapGestureRecognizer(target: self, action: #selector(touchCover(_:)))
            coverTapRecognizer.numberOfTapsRequired = 1
            coverImageView.tag = index
            coverImageView.isUserInteractionEnabled = true
            coverImageView.addGestureRecognizer(coverTapRecognizer)
        }
    }
```

각 이미지뷰가 몇 번째 커버 이미지인지 가지고 있어야 해서,  
TapGestureRecognizer를 붙여줄 때 coverImageView의 idx를 coverImageView의 tag로 붙여줬다.
이는 다음에 handler 함수에서 sender.view가 imageView가 되므로 sender.view.tag로 참조할 수 있게 된다.



handler 함수도 만들어준다.

```swift
    // MARK: - @IBAction Functions
    
    @objc func touchCover(_ sender: UITapGestureRecognizer) {
        imagePicker.sourceType = .photoLibrary
        guard let senderTag = sender.view?.tag else { return }
        currentPickerIndex = senderTag
        present(imagePicker, animated: true, completion: nil)
    }
```

이 부분이 UIImagePickerController를 띄우는 부분인데,  
띄울 때 트리거된 UITapGestureRecognizer의 tag값을 현재 피커의 인덱스값을 저장해놓는 전역변수  
`currentPickerIndex`에 넣어준다.

```swift
// MARK: - UIImagePickerControllerDelegate & UINavigationControllerDelegate
extension CoverUserPhotoViewController: UIImagePickerControllerDelegate, UINavigationControllerDelegate  {
    
    // didFinishPickingMediaWithInfo
    func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
        // 선택한 이미지 커버 이미지뷰의 이미지로 변경
        if let image = info[UIImagePickerController.InfoKey.originalImage] as? UIImage {
            coverImageViews[currentPickerIndex].image = image
        }
        dismiss(animated: true, completion: nil)
    }
}
```

extension에 구현해둔 `didFinishPickingMediaWithInfo` 함수의 내용도 채워준다.  
이 때, info는 딕셔너리인데,  
info를 print해 보니까

```
[__C.UIImagePickerControllerInfoKey(_rawValue: UIImagePickerControllerReferenceURL): assets-library://asset/asset.JPG?id=106E99A1-4F6A-45A2-B320-B0AD4A8E8473&ext=JPG, __C.UIImagePickerControllerInfoKey(_rawValue: UIImagePickerControllerMediaType): public.image, __C.UIImagePickerControllerInfoKey(_rawValue: UIImagePickerControllerImageURL): file:///Users/choyi/Library/Developer/CoreSimulator/Devices/61DF3D8E-5E65-422A-B845-BC6E740AFC91/data/Containers/Data/Application/4F57E11F-5AEE-44D5-B8DB-A134D9DDF8CF/tmp/FDAA3411-9412-4539-844A-3699C54794A5.jpeg, __C.UIImagePickerControllerInfoKey(_rawValue: UIImagePickerControllerOriginalImage): <UIImage:0x600003c1c000 anonymous {4288, 2848}>]
```

요런 로그가 찍힌다.

<img src="https://user-images.githubusercontent.com/28949235/133979520-9cbe4b13-0ed5-4ece-bc47-33449785e504.png" alt="image" width=400 />

키값으로는 요런 것들이 있다고 한다!

### info.plist 에서 권한 요청하기

![image](https://user-images.githubusercontent.com/28949235/133975992-078ccfe1-ff75-4ed4-8a79-2c7c1c4812d4.png)

이렇게 두 개 추가해준다.

<img src="https://user-images.githubusercontent.com/28949235/133980808-dfa50082-e2f4-4ae9-b355-5a668111306e.gif" alt="Screen-Recording-2021-09-20-at-6 21 46-PM" width=300px />

잘 된다.



```swift
extension CoverUserPhotoViewController: UIImagePickerControllerDelegate, UINavigationControllerDelegate {
    
    // didFinishPickingMediaWithInfo
    func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey: Any]) {
        
        // 선택한 이미지 커버 이미지뷰의 이미지로 변경
        if let image = info[UIImagePickerController.InfoKey.originalImage] as? UIImage {
            coverImageViews[currentPickerIndex].image = image
            selectedCoverImages[currentPickerIndex] = image
        }
        dismiss(animated: true, completion: nil)
    }
}
```

`didFinishPickingMediaWithInfo` 함수에 나중에 서버로 이미지 배열을 보내기 위해  
`selectedCoverImages[currentPickerIndex] = image` 추가하면 끝!

