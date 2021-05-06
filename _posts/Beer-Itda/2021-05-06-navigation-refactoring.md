---
layout: post
title: "네비게이션 컨트롤러 리팩토링" 
date: 2021-05-06
category: read 
excerpt: ""

---

# 네비게이션 컨트롤러 리팩토링

![Untitled_Artwork 3](https://user-images.githubusercontent.com/28949235/117266737-8849e580-ae90-11eb-8cf3-47285d5f22de.png)

UISearchController를 작업하려고 보니... 처음에 SceneDelegate에서 만든 Navi bar를 쓰려니까  
데이터 통신도 그렇고 delegate 등에서 다 ~~~ 귀찮아지길래  
navi bar를 분리하기로 결정했다.

처음 온보딩에서 tab bar로 넘길 때 present를 해서 해당 nav를 사실상 없애고(?)  
그 다음에 tabbar에서 storyboard reference로 넘길 VC에 navigation controller를 embed in 해준다.

그러면 탭별로 navi stack이 분리된다 당연히.. 그게 훨씬 편할 것 같다.

이렇게 되면 첫 앱 실행 (온보딩O)을 제외하고는 SceneDelegate에서 navi bar를 만들어 줄 필요가 없다.  
왜냐면 어차피 첫 실행 아니면 tabbar-main이 첫 뷰이기 때문에... 이것도 수정해야 할 듯 ~.~