---
layout: post
title: "iOS 프로젝트에서 Firebase로 할 수 있는 것들은 뭐가 있을까?" 
date: 2021-09-20
category: read 
excerpt: ""

---

# iOS 프로젝트에서 Firebase로 할 수 있는 것들은 뭐가 있을까?

## 🦭 Question

개발하고 있는 프로젝트에서  Firebase를 연동하는데,
 검색 결과가 없을 때 뷰에 뜨는 버튼을 클릭 시 Firebase log에 남기도록 하기로 했다. 그 외 FCM이나 Crash log 말고, 더 자세히! Firebase를 사용하면 뭘 할 수 있을지!

파베 뽕 뽑기 프로젝트...

## 🛼 Answer

### **Firebase란?**

구글에서 제공하는 모바일 및 웹 어플리케이션 개발 플랫폼

### DB

NoSQL 데이터 베이스이다. 데이터 베이스 만들기, 클라우드(Cloud Firestore) 생성 등이 가능하다. 개발단계에서 테스트 용도로 클라이언트에서 자유롭게 읽고 쓸 수 있는 권한도 부여할 수 있다고 한다. 뷰가 처음에 로드될 때 데이터를 받아오고, 사용자가 DB를 업데이트 할 수도 있다. 예제 튜토리얼은 [여기](https://yagom.net/forums/topic/firebase-사용해-보기/)서 확인!

raywenderlich에 올라온 [파베 튜토리얼](https://www.raywenderlich.com/21133526-firebase-tutorial-getting-started)도 있다.

### A/B Test

![image](https://user-images.githubusercontent.com/28949235/133987440-39dec3dd-4090-4353-9a15-0a963bf716e9.png)

디지털 환경에서 전체 실사용자를 대상으로 대조군과 실험군으로 나눠서 특정 UI, 알고리즘 등의 효과를 비교할 수 있다. ( 인스타그램, 페이스북 같은 앱에서 업데이트 전 특정 사용자만 UI가 업데이트 된다던가.,, 하는 것들인듯 하다)

조건에 따라 배포 비율도 설정할 수 있다. 예제 튜토리얼은 [여기](https://www.raywenderlich.com/20974552-firebase-tutorial-ios-a-b-testing)서 확인!

### Crash report

앱이 터졌을 경우 해당 crash log를 기록해둘 수 있다. crash report는 커스텀이 가능한데, 아래 4개의 logging mechanisms(??)이 있다고 한다.

1. **Custom key**
2. **Custom logs**
3. **User Identifiers**
4. **Non — fatal exceptions**

예제 튜토리얼은 [여기](https://medium.com/@paulsoham/firebase-crashlytics-in-ios-swift-1d8c9aec63d0)서 확인!

### Analytics

파베의 꽃...(?) 내가 파베를 알게 된 이유 중 하나. 앱 분석을 위해 기획자들이 설치 해 달라고 하긴 했었는데, 자세히 뭘 분석할 수 있는지 궁금했다.

**앱에서 일어나는 일 수집하기**

- 어떤 화면을 거쳐서 구매로 이어지는지
- 화면 내에 어떤 요소들을 많이 선택하는지
- 어떤 화면에서 이탈이 많이 발생하는지
- 어떤 요소를 선택한 유저가 구매전환율이 높은지 등

요런 일들을... 수집할 수 있다고 한다. 더 자세히 보자면,

애널리틱스 툴에서는 크게 3가지 측면에서 분석이 가능한데

- **이벤트**: 고객이 어떤 행동을 할 때 발생하는 데이터(앱 실행, 구매, 버튼 선택 등)

- **사용자 속성**: 잠재고객을 보기위한 필터로서 기본 사용자 속성은 인구통계학적(지역, 성별, 나이등)으로 자동으로 수집되고 최대 25개까지 사용자 속성 생성 가능(재구매 고객 등)

- 잠재고객

  : 사용자 속성을 통해서 고객군을 생성할 수 있음

  - ex. 각 레벨별 고객군: 로그인 시 해당 고객의 레벨을 이벤트로 수집한다. 사용자 속성을 특정레벨 달성자로 구분한다.

이벤트 같은 건 정의를 하나하나 해 줘야 하는 듯 하고, 더 쉽게 하려면 빅쿼리를 공부하면 된다고...

**page view,** 즉 사용자가 어떤 화면에 많이 접속하고, 얼마나 접속하는 지 이런것도 수집할 수 있는데 이것도 마찬가지로 화면에 대한 정의가 필요하다고 한다.

**Funnel event (이동경로)** 같은 것도 수집할 수 있는데, Big Query(유료)를 사용하지 않는 이상 애매하게 수집된다

이런 기획단(?) UX ,, 요쪽을 엄청 세세한 정도까지 수집 및 분석할 수 있다.

자세한 내용은 [여기](https://medium.com/@cute3681/4-firebase-analytics-살펴보기-2818fa683cb5)에!

### 자동으로 수집되는 이벤트 (애널리틱스)

파베 문서에 [자동으로 수집되는 이벤트 리스트](https://support.google.com/firebase/answer/9234069?visit_id=637665242270722933-1085911871&rd=1)가 있긴 한데, 안드로이드만 수집되는 것들이 좀 있다. 자세한건 리스트 확인! 매개변수명도 친절하게 적혀있다.

### 커스텀 이벤트 만들어서 로깅하기

현재 개발하고 있는 프로젝트에서 검색 결과가 없을 때 문의하기 버튼을 누르면
 파베에 log를 찍기로 했는데, 이를 위해서는 커스텀 이벤트를 정의하고 이벤트를 로깅해야 한다.

이를 위한 [블로그 글 및 튜토리얼 링크](https://sweetcoding.tistory.com/53)!

추가로, 연동하는 법을 잘 설명해준 [수진언니 블로그 링크](https://sujinnaljin.medium.com/ios-firebase-파이어베이스-연동-56bcc972ec8f)도 첨부,,
