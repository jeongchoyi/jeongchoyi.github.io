---
layout: post
title: "별점 슬라이더 만들기" 
date: 2021-06-22
category: read 
excerpt: ""

---

## 별점 슬라이더 만들기

![image](https://user-images.githubusercontent.com/28949235/122777263-d3a44000-d2e6-11eb-9de7-6cb37cdd7fa6.png)

0.5 단위로 슬라이드 되는 별점 슬라이더 만들기!

![image](https://user-images.githubusercontent.com/28949235/122778021-912f3300-d2e7-11eb-870e-2e8baea59d7f.png)

우선 슬라이드가 되어야 하니까 slider를 올려준다.

![image](https://user-images.githubusercontent.com/28949235/122777662-35fd4080-d2e7-11eb-9abb-17b87ed10d56.png)

별점 이미지를 UIImageView로 5개 만들어주고 stack view로 감싸준다.

![image](https://user-images.githubusercontent.com/28949235/122778426-f420ca00-d2e7-11eb-9fed-8fa10e5bd8a2.png)

![image](https://user-images.githubusercontent.com/28949235/122778495-0438a980-d2e8-11eb-8984-ecd6db0d61c1.png)![image](https://user-images.githubusercontent.com/28949235/122778517-08fd5d80-d2e8-11eb-90c2-7273cd8f1d6c.png)

(왼: slider, 우: stack view)  
별점 stack view에서 slider로 위와 같이 constraint를 잡아준다. (겹치게끔)

이제 slider가 slide 될 때 마다 별점 이미지 뷰의 이미지를 바꿔주면 되는데, 이를 위해  
UISlider의 Delegate 함수를 뒤져보자,,!!

[Docs](https://developer.apple.com/documentation/uikit/uislider)

보니까 Delegate는 따로 없고...

![image](https://user-images.githubusercontent.com/28949235/122778991-70b3a880-d2e8-11eb-89ab-5a8a57455b18.png)

요런 부분이 있다.  
addTarget 함수 써서 value changed event 감지해서 selector 함수 등록해서 쓰라는 얘기인 듯!

![image](https://user-images.githubusercontent.com/28949235/122779462-dacc4d80-d2e8-11eb-8e3a-9c0a9ecff209.png)

@IBOutlet 연결해주고

![image](https://user-images.githubusercontent.com/28949235/122779482-def86b00-d2e8-11eb-9fc6-53a7ac1f48cd.png)

addTarget 해준다.  
이제 `slideStarSlider()` 함수 내부에서 slide될 때마다 slider의 value값에 따라  
이미지 처리 등을 해주면 된다!

![image](https://user-images.githubusercontent.com/28949235/122779686-0f400980-d2e9-11eb-9d2e-476fca58026a.png)

slider의 min, max 값 설정 해주고

![image](https://user-images.githubusercontent.com/28949235/122780049-6e9e1980-d2e9-11eb-9ae3-ef574421a8df.png)

switch문으로 분기처리 해 줬다.  
이제 stack view내 이미지뷰들을 가져와서 이미지만 변경해주면 된다.

![image](https://user-images.githubusercontent.com/28949235/122782283-8d051480-d2eb-11eb-8a91-968c921a6c23.png)

UIImageView 배열을 만들어주고

![image](https://user-images.githubusercontent.com/28949235/122782305-92625f00-d2eb-11eb-8e07-24530aa488b8.png)

for문을 돌려서 starStackView의 subview들을 append 해준다.

![image](https://user-images.githubusercontent.com/28949235/122785490-64cae500-d2ee-11eb-916d-5f6feec45eff.png)

selector 함수 로직은 이렇게!  
value가 0.5~1 사이면 한칸이 칠해져야 하므로 ... 고게 좀 헷갈렸다.

![image](https://user-images.githubusercontent.com/28949235/122785660-84620d80-d2ee-11eb-8565-3ecd235581ed.png)

slider를 stack view보다 위로 올려주고,

![image](https://user-images.githubusercontent.com/28949235/122785687-89bf5800-d2ee-11eb-9fdb-52960b6fc3a2.png)

alpha값을 0으로 줘서 투명하게 만들면 완성! 되나 했는데  
alpha값을 0으로 줘버리면 hidden처리가 되는건지 터치가 안먹는다.

![image](https://user-images.githubusercontent.com/28949235/122786231-0e11db00-d2ef-11eb-9eef-af5171afcfb0.png)

thumb 이미지 투명으로 바꿔주고

![image](https://user-images.githubusercontent.com/28949235/122786244-12d68f00-d2ef-11eb-8ef5-4b45d8929dc7.png)

alpha는 0.1로, Tint color도 투명하게 바꿔줬다

![Screen-Recording-2021-06-22-at-12 15 17-AM](https://user-images.githubusercontent.com/28949235/122786335-2aae1300-d2ef-11eb-8275-4f5c3bc27207.gif)

끝 ~
