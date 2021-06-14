---
layout: post
title: "ViewController의 생명주기" 
date: 2021-06-02
category: read 
excerpt: ""

---

## ViewController의 생명주기

<img src="https://user-images.githubusercontent.com/28949235/120156210-ba9af880-c22c-11eb-9f2b-d767966a017d.png" alt="image" width=400px /><img src="https://user-images.githubusercontent.com/28949235/120156384-e61de300-c22c-11eb-80d6-7c3e6cfcb7f5.png" alt="image" width=300px />

* **viewDidLoad**
  * 뷰 컨트롤러가 메모리에 로드된 후 호출
  * 시스템에 의해 자동으로 호출됨
  * 리소스 추가 / 초기화면 구성
  * 화면이 처음 만들어질 때 한 번만 실행
* **viewWillAppear**
  * 뷰가 계층에 추가되고 화면이 표시되기 직전에 호출
  * 화면이 나타날 때 마다 수행해야 하는 작업 수행
  * 다른 뷰로 갔다가 다시 돌아오는 상황에 해주고 싶은 작업 수행
* **viewDidAppear**
  * 뷰가 계층에 추가되어 화면이 표시된 직후에 호출
  * 뷰를 나타내는 것과 관련된 추가적인 작업 수행
  * 화면에 적용될 애니메이션을 그려줌
* **viewWillDisappear**
  * 뷰가 계층에서 사라지기 직전에 호출
  * 뷰가 생성된 뒤 발생한 변화를 이전 상태로 되돌리기 좋은 시점
* **viewDidDisappear**
  * 뷰가 계층에서 사라진 후 호출
  * 시간이 오래 걸리는 작업은 권장되지 않음

> 다른 뷰로 push했을 때, 다른 뷰의 viewDidLoad 전 원래 뷰의 viewWillDisappear가 호출되고,  
> 다른 뷰의 viewWillDisappear가 호출되고 나서야 원래 뷰의 viewDidDisappear가 호출 됨  
> 그 다음에서야 다른 뷰의 viewDidAppear가 호출 됨

> SOPT 28기 iOS 파트 1차 세미나 내용 中

