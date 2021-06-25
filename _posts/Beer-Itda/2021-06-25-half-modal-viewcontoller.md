---

layout: post
title: "half-modal 하프모달 뷰 컨트롤러 만들기" 
date: 2021-06-25
category: read 
excerpt: ""

---

## half-modal 하프모달 뷰 컨트롤러 만들기

<img src="https://user-images.githubusercontent.com/28949235/123397784-c84e5e80-d5dd-11eb-9148-aad5051bd16d.png" alt="image" width=600px />

요런 하프모달 뷰를 띄우고, navi push까지 해보려고 한다!

### Half Modal 띄우기

그냥 present를 하는 게 아니라, `UIPresentationController`를 써먹어야 한다

```swift
import UIKit

class HalfModalPresentationController: UIPresentationController {
    
    let blurEffectView: UIVisualEffectView!
    var tapGestureRecognizer: UITapGestureRecognizer = UITapGestureRecognizer()
    var check: Bool = false
    
    override init(presentedViewController: UIViewController, presenting presentingViewController: UIViewController?) {
        let blurEffect = UIBlurEffect(style: .dark)
        blurEffectView = UIVisualEffectView(effect: blurEffect)
        super.init(presentedViewController: presentedViewController, presenting: presentedViewController)
        tapGestureRecognizer = UITapGestureRecognizer(target: self, action: #selector(dismissController))
        blurEffectView.autoresizingMask = [.flexibleWidth, .flexibleHeight]
        self.blurEffectView.isUserInteractionEnabled = true
        self.blurEffectView.addGestureRecognizer(tapGestureRecognizer)
    }
    
    override var frameOfPresentedViewInContainerView: CGRect {
        CGRect(origin: CGPoint(x: 0,
                               y: self.containerView!.frame.height * 181 / 800),
               size: CGSize(width: self.containerView!.frame.width,
                            height: self.containerView!.frame.height * 619 / 800))
    }
    
    // 모달이 올라갈 때 뒤에 있는 배경을 검은색 처리해주는 용도
    override func presentationTransitionWillBegin() {
        self.blurEffectView.alpha = 0
        self.containerView!.addSubview(blurEffectView)
        self.presentedViewController.transitionCoordinator?.animate(alongsideTransition: { _ in self.blurEffectView.alpha = 0.7},
                                                                    completion: nil)
    }
    
    // 모달이 없어질 때 검은색 배경을 슈퍼뷰에서 제거
    override func dismissalTransitionWillBegin() {
        self.presentedViewController.transitionCoordinator?.animate(alongsideTransition: { _ in self.blurEffectView.alpha = 0},
                                                                    completion: { _ in self.blurEffectView.removeFromSuperview()})
    }
    
    // 모달의 크기가 조절됐을 때 호출되는 함수
    override func containerViewDidLayoutSubviews() {
        super.containerViewDidLayoutSubviews()
        blurEffectView.frame = containerView!.bounds
        self.presentedView?.makeRoundedSpecificCorner(corners: [.topLeft, .topRight], cornerRadius: 15)
    }
    
    @objc func dismissController() {
        self.presentedViewController.dismiss(animated: true, completion: nil)
    }
}

```

이렇게 `HalfModalPresentationController` 클래스를 만들어 주고,  
모달을 present하는 곳에서

```swift
private func presentReviewModalViewController() {
        let reviewModalStoryboard = UIStoryboard(name: Const.Storyboard.Name.reviewModal, bundle: nil)
        guard let reviewModalViewController = reviewModalStoryboard.instantiateViewController(withIdentifier: Const.ViewController.Identifier.reviewModal) as? ReviewModalViewController else {
            return
        }
            
        reviewModalViewController.modalPresentationStyle = .custom
        reviewModalViewController.transitioningDelegate = self
        present(reviewModalViewController, animated: true, completion: nil)
}
```

요런식으로 해 주면 되는데,  
내가 짜는 뷰 같은 경우에는 <img src="https://user-images.githubusercontent.com/28949235/123399345-77d80080-d5df-11eb-9050-a898b31b2aae.png" alt="image" width=100px /> 이 버튼을 눌렀을 때 등급 가이드 뷰로 push해야 해서

```swift
private func presentReviewModalViewController() {
        let reviewModalStoryboard = UIStoryboard(name: Const.Storyboard.Name.reviewModal, bundle: nil)
        guard let reviewModalViewController = reviewModalStoryboard.instantiateViewController(withIdentifier: Const.ViewController.Identifier.reviewModal) as? ReviewModalViewController else {
            return
        }
        let reviewModalNavigationController = UINavigationController(rootViewController: reviewModalViewController)
            
        reviewModalNavigationController.modalPresentationStyle = .custom
        reviewModalNavigationController.transitioningDelegate = self
        present(reviewModalNavigationController, animated: true, completion: nil)
}
```

이렇게 `reviewModalNavigationController`를 만들어주고,  
`reviewModalNavigationController` 를 half modal로 present해 줬다.

