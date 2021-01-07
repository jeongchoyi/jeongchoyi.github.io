---
layout: post
title: "MOMO iOS 컨벤션, 네이밍 정리" 
date: 2021-01-07
category: read 
excerpt: ""

---

# MOMO iOS 팀 컨벤션, 네이밍 모아보기

> 작업 순서대로 작성 !

## Branch Naming

세분화된 feature 별로 브랜치 생성  
소문자 + 언더바(_) 사용 단어 구분  
View 작업 시에는 제플린 view 이름 적극 활용  

ex) `list_filter_modal`, `home_bubble`

## Coding Convention

[MoMo-iOS Swift Style Guide](https://github.com/Team-MoMo/MoMo-iOS/wiki/MoMo-iOS-Swift-Style-Guide)

\+ SwiftLint 사용

## Commit Type

`feat:` 새로운 기능 추가 (new feature)
`fix:` 버그 수정 (bug fix)
`docs:` 문서 작성, 수정 (documentation)
`style:` 코드 포맷팅, 세미콜론 누락 등 코드 변경이 없는 경우
`refactor:` 코드 리팩토링 (refactoring)
`test:` 테스트 코드, 리팩토링 테스트 코드 추가
`chore:` 빌드 업무 수정, 패키지 매니저 수정 등 (production code 변경이 없는 경우)
`perf:` 성능 개선

## Issue Title

```
[Commit Type] 이슈 제목
```

그리고 **Commit Type 라벨**, **각자 이름 라벨** 추가

## Commit Message Convention

``` 
[Commit Type] 커밋 내용 설명
```

## Pull Request Title

```
[#이슈번호] 커밋 내용 요약
```

## Pull Request Body

```
### Description
캡쳐 첨부, 한글 설명
### Related Issues
Fixes #이슈번호
```



