---
layout: post
title: "대략적인 레이아웃 작성하기" 
date: 2021-03-18
category: read 
excerpt: ""

---

# 대략적인 레이아웃 작성하기

를 하기에 앞서... [새로운 사실](https://iamcho2.github.io/2021/03/17/close-issue-with-commit-message)을 알게 되어서 커밋메세지 컨벤션을 변경했다.

### 대략적인 앱 실행 시나리오 정리하기

> 앱 플로우 완벽 이해를 위해 ~!
>
> 뷰 전환을 중심으로 작성,  
> 뷰 전환이 없는 단순 동작 (ex: 좋아요 누르기)은 표시하지 않음!

![image](https://user-images.githubusercontent.com/28949235/111651967-081ce180-884a-11eb-9465-6fde64715af0.png)

### 최초 실행 분기처리

`UserDefaults.standard.bool(forKey: "didLaunch")` 값으로 분기

### 스토리보드 분리하기

* 로그인 방식 선택, 닉네임 설정
  * 분리할지 Onboarding으로 묶을지 고민...
* 탭바 컨트롤러
  * 스토리보드엔 컨트롤러만 있고 탭바 항목들은 reference로 빼기!
* **메인**
* 메인 코치마크
* 스타일 설정
* 향 설정
* 맥주 상세보기
* 맥주 전체보기
  * 상단에 태그박스 있는 뷰, 없는 뷰가 있다
    * 좋아하는 향(O), 좋아하는 스타일(O), 이런 맥주는 어떠세요?(X)
    * 찜한 맥주(X)
  * 태그박스를 xib로 빼서 뷰의 용도에 따라 뗐다 붙였다 할지 고민해볼까!
* 리뷰 전체보기
* 등급 가이드
* **검색하기**
* 검색 결과 (결과 있을 때)
* 검색 결과 (결과 없을 때)
* **마이페이지**
* 설정
  * 설정 세부 뷰들도!
  * 공지사항, 문의하기, 이용약관, 개인정보 처리방침 등...
* 내가 쓴 리뷰

### 따로 뷰로 작성해야 하는 것들

* 별점, 리뷰 모달
* 맥주 전체보기 태그박스

### cell로 작성해야 하는 것들

도 있는데... 뷰 구성 부터 생각해 봐야 할 것 같다!
