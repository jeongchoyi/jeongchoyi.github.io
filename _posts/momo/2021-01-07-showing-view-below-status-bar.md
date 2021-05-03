---
layout: post
title: "Status bar 아래에 View 보이게 하기" 
date: 2021-01-07
category: read 
excerpt: ""

---

# Status bar 아래에 View 보이게 하기

![image](https://user-images.githubusercontent.com/28949235/103880607-b30f7180-511c-11eb-8a2e-2952f2755e8b.png)

이게 싫어서..

### Safe Area Relative Margins 체크 해제

![image](https://user-images.githubusercontent.com/28949235/103880673-cd494f80-511c-11eb-8147-096ce48ba3a9.png)

가장 상위(?)뷰 누르고

![image](https://user-images.githubusercontent.com/28949235/103880710-dafed500-511c-11eb-8c55-8cadef17e4ed.png)

`Safe Area Relative Margins` 체크를 해제해 준다.

![image](https://user-images.githubusercontent.com/28949235/103880774-ee11a500-511c-11eb-9517-933050803460.png)![image](https://user-images.githubusercontent.com/28949235/103880790-f1a52c00-511c-11eb-8f92-df0e9a64c2a4.png)

Top Constraint를 위와 같이 수정해준다.



![image](https://user-images.githubusercontent.com/28949235/103880804-f5d14980-511c-11eb-8ca7-b546fb1fb8e6.png)![image](https://user-images.githubusercontent.com/28949235/103880818-fa95fd80-511c-11eb-8aee-ddd7e4bb4816.png)

그러면 Constant 값이 52가 되어있는데, 그걸 0으로 맞춰주면 !

![image](https://user-images.githubusercontent.com/28949235/103880949-23b68e00-511d-11eb-826a-68474bc8b437.png)

우히 ^___^

### Bottom도 똑같다

![image](https://user-images.githubusercontent.com/28949235/103881034-4052c600-511d-11eb-871e-421504d92d9b.png)

good