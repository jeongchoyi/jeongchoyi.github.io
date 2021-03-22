---
layout: post
title: "스토리보드 만들기" 
date: 2021-03-20
category: read 
excerpt: ""

---

# 스토리보드 만들기

### 스토리보드, VC 파일 만들고 연결하기

![image](https://user-images.githubusercontent.com/28949235/111866030-cd33be80-89ad-11eb-9fa4-4b8107e6d354.png)

정한 이름들로 스토리보드를 만들었다

![image](https://user-images.githubusercontent.com/28949235/111866034-d15fdc00-89ad-11eb-8810-457ad46d6966.png)

VC 파일들도 만들었다

![image](https://user-images.githubusercontent.com/28949235/111866039-d755bd00-89ad-11eb-8ad7-07f0089bb50a.png)

각 스토리보드에 메인 VC하나씩 만들어 주고,

![image](https://user-images.githubusercontent.com/28949235/111866042-dc1a7100-89ad-11eb-90e6-685ca9639111.png)
![image](https://user-images.githubusercontent.com/28949235/111866045-dfadf800-89ad-11eb-98ca-53c9428b3c95.png)

Class 연결해주고 Initial VC 설정까지 끝!  
Storyborad ID 설정도 해줬다.

### 탭바에 스토리보드 연결하기

![image](https://user-images.githubusercontent.com/28949235/111866110-7b3f6880-89ae-11eb-8c32-23b7c69aa2b5.png)

완료 🐙

### Initial Storyboard 바꿔주기

![image](https://user-images.githubusercontent.com/28949235/111866151-ba6db980-89ae-11eb-8d1f-e69dd36051b1.png)



![image](https://user-images.githubusercontent.com/28949235/111866157-c22d5e00-89ae-11eb-945c-ddc5555ff9b8.png)

이거 두개는 바꿔줘도 계속 Main이 뜬다. (Tabbar가 떠야하는데)

![image](https://user-images.githubusercontent.com/28949235/111866253-8777f580-89af-11eb-8f81-4294d0f85119.png)

요놈을 바꿔줘야 한다!  
근데 어차피 온보딩때문에 나중에 SceneDelegate에서 처리해줘야 함.,,,

차이점이 뭐지? 

### 탭바 아이콘, 타이틀 지정하기

Tab bar Controller를 만들고 Storyboard Refernce를 사용해서 탭바를 구성한 경우에, 

![image](https://user-images.githubusercontent.com/28949235/111866503-41239600-89b1-11eb-988f-3d86a3231230.png)

얘를 아무리 수정해봐도 아무것도 바뀌지 않는다.  
내가 reference로 연결해둔 VC에 가서

![image](https://user-images.githubusercontent.com/28949235/111866490-2c470280-89b1-11eb-9f28-f151e5e01ecc.png)

Tab Bar Item을 추가한 후, 

![image](https://user-images.githubusercontent.com/28949235/111866524-70d29e00-89b1-11eb-9319-27e72b42643a.png)

요놈을 수정해주면 된다.

![image](https://user-images.githubusercontent.com/28949235/111866528-7a5c0600-89b1-11eb-8191-e81473008c0a.png)

시뮬에서 실행시킨 모습!