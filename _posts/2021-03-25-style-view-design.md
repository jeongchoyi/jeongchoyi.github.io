---
layout: post
title: "스타일 선택 뷰 설계하기" 
date: 2021-03-25
category: read 
excerpt: ""

---

# 스타일 선택 뷰 설계하기

스타일 뷰 컴포넌트들을 배치하기 전에 뷰를 어떻게 짤지 설계해보는 시간 ~!

![ㅁㅁ](https://user-images.githubusercontent.com/28949235/112488893-4d4c9080-8dc1-11eb-9b68-364123d6d566.png)

Ale, Lager, Lambic, Etc 부분을 Collection View로 처리할까 고민했는데,  
선택함에 따라 달라지는 아래 뷰에서 pagination이 따로 없다고 해서.... 흠,,

근데 버튼으로 하면 예외처리가 많아질 것 같기도 하고 코드가 지저분해질 것 같다.  
tag를 입혀서 한다고 해도 비슷할 것 같고...

커스텀 탭바를 만들어서 아래는 container view에 집어넣어야 할지도 고민이다.  
버튼으로 하는게 간단(?)하긴 한데 계속 알수없는 찝찝함이 있다 ............

segmented control이 개수가 2개 제한만 아니면 ..! 제일 깔끔할 것 같다.  
![image](https://user-images.githubusercontent.com/28949235/112489808-26428e80-8dc2-11eb-8c58-882811fb0e76.png)

헐 되네 ㅎ

![as](https://user-images.githubusercontent.com/28949235/112489933-41ad9980-8dc2-11eb-918a-7484d5ad4329.png)

우선 땅땅땅!  
뷰를 미리 설계하고 작업하냐 안 하냐의 차이는 굉장한 차이가 있기 때문에...