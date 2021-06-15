---
layout: post
title: "scrollView" 
date: 2021-06-15
category: read 
excerpt: ""

---

# ScrollView

 스크롤 뷰 올릴 때 맨날 헷갈려서...하는 정리

![image](https://user-images.githubusercontent.com/28949235/122045673-fcc15e00-ce18-11eb-8023-129bb167f0d6.png)

height 설정 (대충 크게)

![image](https://user-images.githubusercontent.com/28949235/122045780-1a8ec300-ce19-11eb-995e-ecf3f1b9559c.png)![image](https://user-images.githubusercontent.com/28949235/122045811-2084a400-ce19-11eb-802f-b5f5911e833d.png)

스크롤 뷰 올려주기

![image](https://user-images.githubusercontent.com/28949235/122045860-2e3a2980-ce19-11eb-93a2-15e475d9beae.png)

0,0,0,0 해주고

![image](https://user-images.githubusercontent.com/28949235/122045904-38f4be80-ce19-11eb-94b2-7cb68a09a8fe.png)

Content Layout Guides 체크 해제

![image](https://user-images.githubusercontent.com/28949235/122046007-56c22380-ce19-11eb-90ba-229dbbe54298.png)

UIView 넣어주기

![image](https://user-images.githubusercontent.com/28949235/122046102-748f8880-ce19-11eb-8631-240ca6aca364.png)![image](https://user-images.githubusercontent.com/28949235/122046138-7e18f080-ce19-11eb-8bde-afc56c620185.png)

Equal Widths, Equal Heights 해주기

![image](https://user-images.githubusercontent.com/28949235/122045860-2e3a2980-ce19-11eb-93a2-15e475d9beae.png)

새로 넣은 뷰에도 0,0,0,0

![image](https://user-images.githubusercontent.com/28949235/122046599-10b98f80-ce1a-11eb-83d9-61d761af9c60.png)

이 상태에서

![image](https://user-images.githubusercontent.com/28949235/122046642-22029c00-ce1a-11eb-8dbf-693a26f04a07.png)

width, height 다 Multiplier 1로 설정,  
원하는 스크롤 방향에 따라 Priority 설정
