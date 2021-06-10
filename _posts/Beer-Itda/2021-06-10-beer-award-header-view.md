---
layout: post
title: "Beer Award Header View 그리기" 
date: 2021-06-10
category: read 
excerpt: ""

---

# Beer Award Header View 그리기

<img src="https://user-images.githubusercontent.com/28949235/121467805-c0df6080-c9f4-11eb-938b-cf6c4c33d0b4.png" alt="image" width=400px />

요 부분의 노란 원 부분을 그려봅시다  
이미지 에셋을 받으려다가, 공부도 할 겸 그냥 그려보기로...

### 원 그리기

> radius 주고 border를 주면 되지 않을까?

<img src="https://user-images.githubusercontent.com/28949235/121468021-1b78bc80-c9f5-11eb-8dee-22d4535272f3.png" alt="image" width=400px />

현재 상태... UIView (bgView) 달랑 하나  
이 안에 원을 그릴 UIView들을 총 3개 넣어준다 (원:UIView = 1:1)

<img src="https://user-images.githubusercontent.com/28949235/121468211-61ce1b80-c9f5-11eb-94e3-444bebab149a.png" alt="image" width=400px />

요런 식으로 !

<img src="https://user-images.githubusercontent.com/28949235/121468348-9c37b880-c9f5-11eb-938d-8e6a92a4d566.png" alt="image" width=400px />

다 넣어준 모습!  
이제 다 배경을 Clear로 바꿔주고, @IBOutlet 연결해서 radius, border를 줘보자

<img src="https://user-images.githubusercontent.com/28949235/121468659-241dc280-c9f6-11eb-9d03-47273d5fc0c6.png" alt="image" width=400px />

잘 그려진 모습 ~~ bgView 안에 circleView 들을 넣어줘서 자연스럽게 튀어나간 부분은 가려진다.  
이제 Label, ImageView 등을 배치시키자

### 왼쪽 label 모음부터!

<img src="https://user-images.githubusercontent.com/28949235/121467805-c0df6080-c9f4-11eb-938b-cf6c4c33d0b4.png" alt="image" width=400px />

구현해야 할 뷰를 보면 label 뒤에는 원 stroke가 없는데,

<img src="https://user-images.githubusercontent.com/28949235/121469440-73182780-c9f7-11eb-9ec9-50194391f7e4.png" alt="image" width=400px />

label들을 UIView에 넣고 그 UIView에 배경색을 줘서 구현했다.

<img src="https://user-images.githubusercontent.com/28949235/121469560-ab1f6a80-c9f7-11eb-91d5-6979dc9f0dbe.png" alt="image" width=400px />

짜잔

### 맥주 이미지

그냥 UIImageView 넣어주면 됨

<img src="https://user-images.githubusercontent.com/28949235/121469711-ef126f80-c9f7-11eb-9bed-59d40c7500b4.png" alt="image" width=400px />



## 디테일 잡기

방금까지 살펴본 iPhone 12 Pro 시뮬레이터에서는 예쁘게 잘 보였지만,  
다른 기기에서도

<img src="https://user-images.githubusercontent.com/28949235/121467805-c0df6080-c9f4-11eb-938b-cf6c4c33d0b4.png" alt="image" width=400px />

딱 !! 요렇게 보이려면 각 원들의 크기나 맥주 이미지, auto layout constraints 등을 특정 숫자로 하기보단  
비율로 주는게 더 예쁠 것 같아서...

<img src="https://user-images.githubusercontent.com/28949235/121469711-ef126f80-c9f7-11eb-9bed-59d40c7500b4.png" alt="image" width=400px />

(iPhone 12 Pro)

<img src="https://user-images.githubusercontent.com/28949235/121469840-2aad3980-c9f8-11eb-9398-08e0a7958059.png" alt="image" width=400px />

(iPod 7th Generation)



1. 각 circleView에 height, width constraints를 삭제하고 aspect ratio 1:1 constraints를 준다.

2. <img src="https://user-images.githubusercontent.com/28949235/121470147-b6bf6100-c9f8-11eb-9955-5dd8ea9c07a4.png" alt="image" width=300px />

   각 circleView에서 bgView로도 aspect ratio constraints를 걸어준다.

3. <img src="https://user-images.githubusercontent.com/28949235/121470319-fe45ed00-c9f8-11eb-9812-48e64531681f.png" alt="image" width=400px />

   클릭해서 보면 circleView의 width랑 bgview의 height 비율이 걸려있는데,  
   bgView의 height은 고정이므로 width로 바꿔줘야 한다. (물론 비율값도 바꿔줘야 함)

   <img src="https://user-images.githubusercontent.com/28949235/121470432-2f262200-c9f9-11eb-838b-aff66a59b017.png" alt="image" width=400px />

circleaView.width : bgView.width = 155:324 이므로 155:324 라고 Multiplier 부분에 써준다.  
다른 두 개의 constraints도 마찬가지로!

![image](https://user-images.githubusercontent.com/28949235/121471515-c8096d00-c9fa-11eb-869d-43f1bee71b76.png)

원 크기는 각기 다른 디바이스에서도 비율대로 조절이 잘 되는 모습 !!  
이제 남은 건   
circle view leading, trailing, top, bottom constraints,  
bgView radius,   
imageView 크기와 constraints

### circle view leading, trailing, top, bottom constraints, 조절하기

기기가 달라짐에 따라 원의 어느 부분까지 가려져야 하는데,  
constraints들을 그냥 숫자로 딱 입력해버리면 모든 기기에서 그 숫자만큼 circle view가 이동하기 때문에,  
얼마만큼 가려지는지가 달라지게 된다.

요 부분을 조절해줄 것!

순서대로  
circle1은 top, leading  
circle2는 leading, bottom  
circle3은 top, trailing이 걸려있는데,  이들을 다 @IBOutlet 연결 해주고  
constant를 변경하는 식으로 조절하려고 한다.



constant를 결졍하는 방식은 해당 원이 해당 방향으로 **얼마나 가려져야 하는 지**를 기준으로 했다.  
예를들어 figma에서 63*63인 원이 top방향으로 29만큼 가려져 있으면,  
그 원이 100 * 100인 크기가 되더라도 가려진 비율은 같아야 하니까.

계산은 63:29 = 100:x 뭐 이런식으로 해서 x를 constants로 주면 된다. (x = 29 * 100 / 63)

<img src="https://user-images.githubusercontent.com/28949235/121476185-5254cf80-ca01-11eb-96b9-4ce8c1d06297.png" alt="image" width=350px /><img src="https://user-images.githubusercontent.com/28949235/121476084-33563d80-ca01-11eb-8dc0-54204b764dc4.png" alt="image" width=350px />

순서대로 조절 전, 조절 후.. 티는 크게 안 나지만 차이가 있긴 있다

### bgView radius 조절

26이라고 딱 박을게 아니라 bgView의 높이에 따라 맞춰주면 된다.

figma에서 180일 때 radius가 26이었으니까  
26 : 180 = radius : bgView.frame.height

즉 radius는 ( 26 * bgView.frame.height ) / 180



### imageView 크기와 constraints 조절

위에 했던 것들과 마찬가지의 방법으로 조절해주면 된다.



### 전후비교

<img src="https://user-images.githubusercontent.com/28949235/121469711-ef126f80-c9f7-11eb-9bed-59d40c7500b4.png" alt="image" width=350px /><img src="https://user-images.githubusercontent.com/28949235/121469840-2aad3980-c9f8-11eb-9398-08e0a7958059.png" alt="image" width=350px />

전

![image](https://user-images.githubusercontent.com/28949235/121478453-ff304c00-ca03-11eb-9b8c-588fcc88823c.png)

후

(iPhone 12 Pro, iPod touch 7th Generation)
끝 !!

