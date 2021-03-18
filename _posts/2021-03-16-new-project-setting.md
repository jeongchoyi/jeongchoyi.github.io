---
layout: post
title: "새 iOS 프로젝트 생성하기" 
date: 2021-03-16
category: read 
excerpt: ""

---

# iOS 프로젝트 생성하기

### Identity

![image](https://user-images.githubusercontent.com/28949235/111332913-88f6a480-86b5-11eb-8528-9699bb43ac14.png)

![image](https://user-images.githubusercontent.com/28949235/111334527-f8b95f00-86b6-11eb-88c8-f34178d5e782.png)

* **Bundle Identifier**

  * 나중에 팀 개발자 계정 만들어지면 바꾸겠지만 Organization Identifier가 `com.choyi`여서  
    처음엔 우선 `com.choyi.Beer-Itda`가 되었다..!

  * 문득 Bundle Identifier convention이 있을 것 같아 찾아봤다.  
    딱 정해진 컨벤션은 없고, 보통 lower case를 사용한다고 해서 (apple의 예제에서도)  
    `com.choyi.beer-itda`로 변경했다.  
    (apple 예제에서는 `com.example.apple-samplecode` 으로 사용했다고 한다!)

* **Version**
  * [전 글](https://iamcho2.github.io/2021/03/13/beer-itda-setting)에서 정해놓은 마일스톤 대로는 버저닝을 하지 않을 거지만,  
    배포 이후 버저닝을 위해 `1.0`에서 `1.0.0`으로 변경했다.  
    (직전에 하던 프로젝트도 `1.0`으로 배포했다가 바로 `1.0.0`으로 바꾼 경험이 있다,,,!)


### Deployment Info

![image](https://user-images.githubusercontent.com/28949235/111335985-2ce14f80-86b8-11eb-94af-fa6c71013708.png)

![image](https://user-images.githubusercontent.com/28949235/111335899-1935e900-86b8-11eb-9b0a-c946a015b13b.png)

* **iOS 버전**
  * default값은 14.4인데, 14.1로 내렸다.  
    초기 유저가 있었으면 초기 유저를 고려해야겠지만 (안드로이드 앱은 유저가 있다고 한다)  
    iOS앱은 그런 게 없어서, 많이 내리고 싶진 않았다.. (생각해야 할 것들이 늘어나는 게 싫었다  ◠‿◠ )
  * iPad 체크 해제를 해 두었다.
    * 근데 MOMO 보면 아이패드 앱스토어 업로드랑은 상관 없는 것 같다.  
      iPad 체크 해제 해도 패드에서 잘만 뜨고 잘만 켜지더라구..
* Device Orientation
  * 화면 회전을 막아두었다!
  * Landscape Left, Landscape Right에 체크 해제를 했다.
* Status Bar Style
  * 직전에 진행하던 프로젝트에서 다크모드 때문에 Status Bar 색상 이슈가 있어서,  
    추후에 필요하다면 건드려야 할 항목! 지금은 아직 뷰가 나온게 별로 없어서 보류 ~.~

### 폴더링

![image-20210317005658853](/Users/choyi/Library/Application Support/typora-user-images/image-20210317005658853.png)

* `APIModels` 과 `AppModels`
  * 전 프로젝트를 진행할 때 App 내에서 사용하는 Model과  
    서버에서 받아올 데이터로써의 Model을 분리했다.
  * 좋은 방법 같아서 이번 프로젝트에서도 같은 방식으로 구별하려고 한다!  (필요 시...)