---
layout: post
title: "UIScrollViewDelegate 메소드 파헤치기" 
date: 2020-12-30
category: read 
excerpt: ""

---

# UIScrollViewDelegate 메소드 파헤치기

[Docs](https://developer.apple.com/documentation/uikit/uiscrollviewdelegate) 순으로 ...

## Responding to Scrolling and Dragging 스크롤, 드래그에 반응

![image](https://user-images.githubusercontent.com/28949235/103328485-6b457600-4a9c-11eb-88c3-1d25147821cc.png)

* **scrollViewDidScroll()**
* 스크롤 할 때 마다 계속 호출. 
* offset 값이 바뀔 때 마다 호출되므로,,, 한번 쓰윽 스크롤 해도 우르르르르르 호출됨

![image](https://user-images.githubusercontent.com/28949235/103328571-b52e5c00-4a9c-11eb-8176-2877e125d4dd.png)

* **scrollViewWillBeginDragging()**
* 스크롤 뷰에서 드래그 하기 시작할 때 한 번만 호출
* 시간이 쪼금 걸리거나, 이동거리를 약간 요할 수 있음

![image](https://user-images.githubusercontent.com/28949235/103328639-f292e980-4a9c-11eb-9589-7c9c39b96e07.png)

* **scrollViewWillEndDragging()**
* 손을 뗐을 때 한 번만 호출
* 속도(velocity) = points/millisecond
* **targetContentOffset**을 변경하여 스크롤 뷰가 정지되는 위치를 조정할 수 있음. 헐 나 이거 써야될 걸

![image](https://user-images.githubusercontent.com/28949235/103328706-41d91a00-4a9d-11eb-822a-d1771016fa10.png)

* **scollViewDidEndDragging()**
* 손을 뗐을 때 한 번만 호출 (위 scrollViewWillEndDragging과 동일)
* 스크롤 모션의 감속 여부를 알 수 있음
* decelerate : 손 뗀 후로 계속 이동할 경우 true

![image](https://user-images.githubusercontent.com/28949235/103328801-8e245a00-4a9d-11eb-8fc5-265412a784b2.png)

* **scrollViewShouldScrollToTop()**
* 맨 위 누르면 뷰의 최상단으로 올라가는 거 허용할건지 안 할건지 설정
* 이 함수가 정의되어있지 않으면, default 값은 YES임

![image](https://user-images.githubusercontent.com/28949235/103328850-b744ea80-4a9d-11eb-8d63-ae26ce66a201.png)

* **scrollViewDidScrollToTop()**
* 맨 위 누르면 뷰의 최상단으로 올라가는 이벤트가 발생할 때 호출되는 함수
* 이미 최상단에 위치하고 있으면 즉시 호출될 수 있음

![image](https://user-images.githubusercontent.com/28949235/103328921-f96e2c00-4a9d-11eb-9072-2d0f763b0081.png)

* **scrollViewWillBeginDecelerating()**
* 손을 뗀 이후로!! 감속이 시작됐을 때 호출되는 함수

![image](https://user-images.githubusercontent.com/28949235/103328986-3a664080-4a9e-11eb-9a7d-043c0574ec4b.png)

* scrollViewDidEndDecelerating()
* 감속이 끝났을 때 호출되는 함수 (스크롤 뷰의 움직임이 멈췄을 때 halt 상태일 때! 정지!)

## Managing Zooming

![image](https://user-images.githubusercontent.com/28949235/103329253-4dc5db80-4a9f-11eb-8ff0-8c1d6ebf46b8.png)

* **viewForZooming()**
* ImageView를 확대하고 싶을 때 자주 쓰는 것 같다. [여기](https://g-y-e-o-m.tistory.com/90) 참고
* scale될 뷰를 반환한다. 만약 deletate가 nil을 반환하면 아무 일도 일어나지 않음

![image](https://user-images.githubusercontent.com/28949235/103329299-8cf42c80-4a9f-11eb-97a2-532f1b11dcd4.png)

* **scrollViewWillBeginZooming()**
* scrollView가 그 내용을 zoom 하기 전에 호출됨

![image](https://user-images.githubusercontent.com/28949235/103329381-e52b2e80-4a9f-11eb-9ff8-b01c809c8333.png)

* **scrollViewDidEndZooming()**
* content의 zoom이 끝났을 때 호출됨
* scale between minimum and maximum. called after any 'bounce' animations

![image](https://user-images.githubusercontent.com/28949235/103329463-21f72580-4aa0-11eb-8fc5-5eef6e32621f.png)

* **scrollViewDidZoom()**
* Tells the delegate that the scroll view’s zoom factor changed.

## Responding to Scrolling Animations

![image](https://user-images.githubusercontent.com/28949235/103329509-4521d500-4aa0-11eb-950f-77474fe13885.png)

* **scrollViewDidEndScrollingAnimation()**
* setContentOffset 또는 scrollRectVisible 이 사용될 때 호출되는 함수
* called when setContentOffset/scrollRectVisible:animated: finishes. not called if not animating

## Responding to Inset Changes

![image](https://user-images.githubusercontent.com/28949235/103329638-da24ce00-4aa0-11eb-904f-1772bf1f8f55.png)

* **scrollViewDidChangeAdjustedContentInset()**

* *adjustedContentInset = contentInset + system inset*

* `contentInset`을 변경하거나 content insets가 시스템에 의해 조정되면 
  UIScrollView 및 UIScrollView Delegate의 메소드가 호출 됨.

