---
layout: post
title: "추천 맥주 전체보기 뷰 설계하기" 
date: 2021-04-26
category: read 
excerpt: ""

---

# 추천 맥주 전체보기 뷰 설계하기

![Untitled_Artwork](https://user-images.githubusercontent.com/28949235/116050277-3e077e00-a6b2-11eb-85ab-f7c47b885d1a.png)

셀에서 향(최대 3개) 표시하는 부분은 굳이 Collectionview로 안 빼도 될 것 같아서  
그냥 UIView 3개 Stack View로든 뭐든 붙이고 1개나 2개일 경우엔  
끝에서부터 hidden 시키면 될 것 같다.

물어봐야 되는 건 table view cell touch 영역.  
table view cell에 버튼 넣으면 알아서 그 버튼 영역 제외하고 알아서 잡히는지 모르겠어서  
우선 다 만들고 물어봐야 한다!

