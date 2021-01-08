---
layout: post
title: "ScrollView + TableView 실습" 
date: 2021-01-07
category: read 
excerpt: ""

---

# ScrollView + TableView 실습

![image](https://user-images.githubusercontent.com/28949235/103861685-25725880-5101-11eb-98a5-d13d5753d51a.png)

쭈루루룩 한 뷰에서 스크롤을 통해 저 모든 뷰들이 다 보여야 하는 앱이라서,  
상단의 뷰(Day, Night)와 Bubble Table View를 합쳐보는 작업을 실습해보고,, 기록하려고 한다~~ !!

### ScrollView 만들기

![image](https://user-images.githubusercontent.com/28949235/103877875-01bb0c80-5119-11eb-8003-bc4085d2ee49.png)

한 2000이면 충분하겠지?

![image](https://user-images.githubusercontent.com/28949235/103878008-2c0cca00-5119-11eb-9d3d-9fc025e8237a.png)

뷰컨에 uiScrollView 넣어주고

![image](https://user-images.githubusercontent.com/28949235/103878015-2fa05100-5119-11eb-900a-3c3927037904.png)

`Content Layout Guides` 체크 해제

![image](https://user-images.githubusercontent.com/28949235/103878096-4f377980-5119-11eb-8c1d-325535c8e26c.png)

super view에다가 0,0,0,0 constraints 추가

![image](https://user-images.githubusercontent.com/28949235/103878260-89088000-5119-11eb-9edc-190112434923.png)

UIView 넣어주고 얘도 0,0,0,0 Constraints 추가

![image](https://user-images.githubusercontent.com/28949235/103878390-b6552e00-5119-11eb-86bd-051f263513c1.png)![image](https://user-images.githubusercontent.com/28949235/103878413-bd7c3c00-5119-11eb-9815-41208f0b824a.png)

사진이 곧 내용..

![image](https://user-images.githubusercontent.com/28949235/103878566-f1576180-5119-11eb-92a8-7c3c4c44e4a9.png)![image](https://user-images.githubusercontent.com/28949235/103878580-f9af9c80-5119-11eb-9252-e79cb60e347a.png)

왼쪽 짧은 거 누르고 오른쪽처럼 Priority, Multiplier 설정  
(width도 같은 방식으로 해주는데 Multiplier만 1로 설정)

![image](https://user-images.githubusercontent.com/28949235/103878656-121fb700-511a-11eb-915a-807bc86d782d.png)

그럼 이렇게 된다~!

# Day, Night View 만들기

![image](https://user-images.githubusercontent.com/28949235/103878820-46937300-511a-11eb-8661-6a0ce695efaa.png)

그럼 이제 거기다 Day, Night이 될 View를 만들어 준다.

![image](https://user-images.githubusercontent.com/28949235/103878977-8d816880-511a-11eb-92fe-42cc9479af6a.png)

어차피 Height Constraint는 code로 잡아줄거라.. 대충 설정

# TableView 넣어주기

![image](https://user-images.githubusercontent.com/28949235/103879070-b0ac1800-511a-11eb-9668-22e0195446af.png)![image](https://user-images.githubusercontent.com/28949235/103878977-8d816880-511a-11eb-92fe-42cc9479af6a.png)

얘도 Height Constraint는 code로 잡아줄거라.. 위랑 똑같이! 하지만  
![image-20210107190222033](/Users/choyi/Library/Application Support/typora-user-images/image-20210107190222033.png)

Bottom을 꼭 잡아줘야 된다. scrollView 니까 ~!



사실 여기서 중요한건 이게 아니라 .. 다음편에서 이어집니다 ,,ㅋ

