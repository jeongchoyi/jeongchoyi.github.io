---
layout: post
title: "CGImage resize, crop하기" 
date: 2021-09-24
category: read 
excerpt: ""

---

# CGImage resize, crop하기

<img src="https://user-images.githubusercontent.com/28949235/134613133-ce2086dc-a8e6-4717-91ce-a7e3b008a10b.png" alt="image" width=300 />

한 사진을 업로드하면 비율에 맞게 12분할 후 순서에 맞는 이미지뷰에 넣어야 한다.

사진 업로드는 [저번에 각 표지를 업로드 할 때](https://iamcho2.github.io/2021/09/20/UIImagePickerController)와 똑같이 해준다.

```swift
var imagePicker = UIImagePickerController()
```

`UIImagePickerController` 만들어주고,

``` swift
private func assignDelegate() {
	imagePicker.delegate = self
}
```

delegate 위임해주고,

```swift
@IBAction func touchPopo(_ sender: Any) {
    imagePicker.sourceType = .photoLibrary
    present(imagePicker, animated: true, completion: nil)
}
```

포포 이미지를 클릭했을 때 `UIImagePickerController` 를 띄운다.

```swift
// MARK: - UIImagePickerControllerDelegate & UINavigationControllerDelegate

extension CoverCalendarViewController: UIImagePickerControllerDelegate, UINavigationControllerDelegate {
    
    // didFinishPickingMediaWithInfo
    func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey: Any]) {

        if let image = info[UIImagePickerController.InfoKey.originalImage] as? UIImage {
          
        }
        dismiss(animated: true, completion: nil)
    }
}

```

extension도 만들어준다.



이렇게까지 하면 사진은 업로드가 되는데, 이제 남은 건 사진 resizing하고 분할하기 !

![제목_없는_아트워크 9](https://user-images.githubusercontent.com/28949235/134627662-018bb02e-bc28-4489-94ad-5ca001cb8d5a.png)

계획은 이거다.

### CGImage

사실 cropping을 먼저 알아봤었는데, cropping함수는 CGImage만 사용가능하다.

UIImage를 CGImage로 바꿔줄 땐 `myImage.cgImage` 를 사용하고,  
CGImage를 UIImage로 바꿔줄 땐 `UIImage(cgImage: myImage)` 를 사용하면 된다.

### 이미지 resizing하기

사용자가 업로드한 사진의 가로, 세로를 비교해서 비율에 맞게 resize해야 한다.  
aspect fill을 구현하면 된다.

> AVFoundation을 import하면 `AVMakeRectWithAspectRatioInsideRect()`를 사용할 수도 있는 것 같다.

CGImage를 쓰고싶어서

![image](https://user-images.githubusercontent.com/28949235/134639079-bb5476a9-9ccf-406f-a1cf-6e7cb6a1dc84.png)

를 써보다가... 이미지가 resizing이 안 되고 애매하게 crop되는 현상이 발생해서 
그냥 UIImage를 사용하는 쪽으로 변경했다.   
그래도 이후 crop할 땐 CGImage를 써야 해서, CGImage로 변환 후 return하는 함수를 만들어줬다.

```swift
// image를 원하는 크기로 resizing
func resizeImage(image: UIImage, size: CGSize) -> CGImage {
    UIGraphicsBeginImageContext(size)
    image.draw(in:CGRect(x: 0, y: 0, width: size.width, height:size.height))
    let renderImage = UIGraphicsGetImageFromCurrentImageContext()
    UIGraphicsEndImageContext()
        
    guard let resultImage = renderImage?.cgImage else {
        print("image resizing error")
        return UIImage().cgImage!
    }
    return resultImage
}
```



이제 `didFinishPickingMediaWithInfo` 함수 안에 계획했던 순서대로 동작을 써 주면 되는데,

우선 필요한 변수들을 만들어준다.

```swift
let originalSize: CGSize = image.size
// 보장되어야 하는 최소 width, height값
let minimumSize: CGSize = CGSize(
width: coverImageView3.frame.origin.x + coverImageView3.frame.width - 	coverImageView1.frame.origin.x,
height: bgStackView.frame.height)
var dividedImages: [CGImage] = []
// 사진의 비율에 맞춰 resize될 CGSize
var newSize: CGSize = CGSize(width: 0, height: 0)
```



그리고 newSize를 지정해준다. (가로, 세로 비교 후)

```swift
// 이미지 resizing을 위한 newSize 지정
// 가로가 더 긴 이미지, 정방형 이미지
if originalSize.width >= originalSize.height {
                
		newSize.width = minimumSize.height * originalSize.width / originalSize.height
		newSize.height = minimumSize.height
                
// 세로가 더 긴 이미지
} else {
                
		newSize.width = minimumSize.width * originalSize.height / originalSize.height
		newSize.height = minimumSize.height
                
}
```



지정해준 newSize대로 이미지를 resizing 해 준다.

```swift
// 이미지 resizing
var resizedImage = resizeImage(image: image, size: newSize)
```



그 후, 그림에서 민트색 사각형대로 먼저 크게 crop해 준다.  
(중앙에 맞춰서 crop해 줄거라 rect도 만들어준다.)

```swift
// 이미지 cropping - 중앙 crop
let centerRect = CGRect(
		x: CGFloat(resizedImage.width / 2 - Int(minimumSize.width) / 2),
		y: CGFloat(resizedImage.height / 2 - Int(minimumSize.height) / 2),
		width: minimumSize.width,
		height: minimumSize.height)
            
resizedImage = resizedImage.cropping(to: centerRect)!
```



그 다음에 이미지를 12개로 잘라준다.

```swift
// 이미지 cropping - 12분할
for idx in 0..<12 {
		let dividedImage = resizedImage.cropping(to: imageRects[idx])
		dividedImages.append(dividedImage!)
}
```



그 후 각 위치에 맞는 imageView에 쏙쏙 넣어주면 끝!

```swift
// 각 위치에 맞는 imageView에 Image 넣기
for (index, image) in dividedImages.enumerated() {
		coverImageViews[index].image = UIImage(cgImage: image)
}
```

![image](https://user-images.githubusercontent.com/28949235/134646949-6b573cb3-54a3-4947-9c92-1f0004b37692.png)

원본 사진은 이거고, 업로드 하면

<img src="https://user-images.githubusercontent.com/28949235/134646766-70b9a716-56fe-4231-9292-e64b3d821d30.png" alt="simulator_screenshot_B6146E0B-8F99-46B9-B9BF-40C6BEAFC336" width=300 />

잘 들어간다.
