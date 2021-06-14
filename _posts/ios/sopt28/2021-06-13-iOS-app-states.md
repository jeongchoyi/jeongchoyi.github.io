---
layout: post
title: "iOS 앱 상태변화, Launch cycle" 
date: 2021-06-13
category: read 
excerpt: ""

---

* ## iOS App States 앱 상태변화

  <img src="https://user-images.githubusercontent.com/28949235/121846204-9dcced80-cd21-11eb-996e-fa01dd78b171.png" alt="image" width=400px />

  **foreground & background**

  * foreground
    * 사용자가 보고 있는 화면
    * CPU를 비롯한 시스템 자원의 우선순위가 높은 상태
  * background
    * 앱이 사용자에게 보이지 않는 상태
    * ex: 앱 인디케이터를 올려 홈 화면으로 이동했을 때 앱의 상태

  

  **애플의 가이드라인**

  * 앱의 상태 변화에 따라 적절히 대응할 것
    * 그렇지 않으면 데이터 손실, 사용자에게 좋지 않은 경험 제공 가능성 O
  * 앱의 상태가 background로 변할 때, 앱이 이에 대해 적절하게 대응해야 함
  * 앱의 시스템 변경 사항을 보고하는 알람을 등록하는 것을 권장
    * 앱이 중지된다면 시스템 큐에 들어갔다가 다시 실행하게 됨

  

  **state 살펴보기**

  * Not running
    * 실행되지 않거나 종료된 상태
  * Inactive
    * 앱이 전면에서 실행 중이지만, 아무런 이벤트를 받고 있지 않는 상태
  * Active
    * 앱이 전면에서 실행 중이며, 이벤트를 받고 있는 상태
  * Background
    * 앱이 background에 있지만 여전히 코드가 실행되고 있는 상태
    * ex: 파일 다운로드, 파일 업로드, 연산처리 등
    * 여분의 실행 시간이 필요한 앱일 경우 특정시간 동안 background 상태로 남아있게 됨
  * Suspended
    * 앱이 보이지 않고 실행되는 코드도 없는 상태
    * 메모리에 유지는 되고 있음
      * But, iOS 시스템은 메모리가 부족하면 Suspend 상태에 있는 앱들을 정리함
    * ex: 다른 앱을 사용하다가 원래 앱으로 돌아왔을 때, 처음부터 앱이 실행되었다면  
      Suspend 상태에 들어갔다가 메모리 부족으로 앱이 정리된 후 처음부터 실행된 것



## App Launch Cycle

> App이 실행되면 시스템은 앱의 메인 함수를 메인 스레드에서 호출하기 위해  
> 프로세스와 메인 스레드를 생성하게 됨
>
> Xcode 프로젝트의 메인 함수는 UIKit Framework를 제어할 수 있게 해줌

<img src="https://user-images.githubusercontent.com/28949235/121846765-817d8080-cd22-11eb-9bab-bce8384daad2.png" alt="image" width=700px />

## 

> SOPT 28기 iOS 파트 1차 복습 자료 내용 中

