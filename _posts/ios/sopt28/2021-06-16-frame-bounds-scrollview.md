---
layout: post
title: "frame, bounds, scrollview" 
date: 2021-06-16
category: read 
excerpt: ""

---

## frame, bounds

### frame

> **Superview(상위 뷰)의 좌표시스템 안에서** View의 위치와 크기를 나타내는 것

### bounds

> **자신만의 좌표 시스템 안에서** View의 위치와 크기를 나타내는 것

* ScrollView에서 중요한 요소!
* 사용자가 손가락으로 스크롤 하는 것 = scrollView의 bounds 값을 변경하는 것

## ScrollView

<img src="https://user-images.githubusercontent.com/28949235/122188252-bf1d0d80-ceca-11eb-8153-af51a6f95a98.png" alt="image" width=500px />

* Content View : 실제 ScrollView에서 보여지는 화면
* Content Inset : ContentVIew 내부의 padding 값을 설정할 수 있는 옵션
* Indicator Inset : 우측 및 하단 스크롤바의 padding을 수정할 수 있는 옵션

<img src="https://user-images.githubusercontent.com/28949235/122188469-f1c70600-ceca-11eb-9166-6b602ec09aae.png" alt="image" width=300px />

* Content Offset : Bounds의 Origin Point
  * Content Offset이 변한다는 것 = 스크롤을 한다는 것
  * Content Offset 값을 활용해 스크롤 되었을 때 무슨 동작을 할 지 처리할 수 이씅ㅁ
  * 좌표값을 넣어 contentOffset 값을 변경 할 수 있음



> SOPT 28기 iOS 파트 2차 복습 자료 내용 中

