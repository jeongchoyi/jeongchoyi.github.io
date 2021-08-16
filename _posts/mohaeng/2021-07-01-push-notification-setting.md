---
layout: post
title: "푸쉬알림 환경설정하기" 
date: 2021-07-01
category: read 
excerpt: ""

---

# 푸쉬알림(APNs) 환경설정하기

> 내 개발자 계정 생기기 전 까지는 동아리에서 제공하는 공용 계정을 사용할 예정이라서  
> 토큰 기반 연결 설정은 사용 못하고 (계정 당 2개 발급 가능)  
> APNs에 대한 인증서 기반 연결 설정을 하려고 한다.

[여기](https://hsdev.tistory.com/entry/iOS-%EC%95%B1-%EB%B0%B0%ED%8F%AC-4-%EC%95%B1%EC%8A%A4%ED%86%A0%EC%96%B4-%EC%BB%A4%EB%84%A5%ED%8A%B8%EC%97%90-%EC%8B%A0%EA%B7%9C-%EC%95%B1-%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0) 따라하면서 쭉쭉 진행해준다.

![image](https://user-images.githubusercontent.com/28949235/124350292-d11ee000-dc2e-11eb-95e6-1532a93c6f81.png)

Configutaion - 내가 만든 Configuration 에 들어가서 Edit을 누른다.

![image](https://user-images.githubusercontent.com/28949235/124350315-f7448000-dc2e-11eb-8323-c6682401b20d.png)

요런 창이 뜨는데 Create Certificate를 눌러준다. (위에껀 개발 중 사용, 아래껀 배포 후 사용)

![image](https://user-images.githubusercontent.com/28949235/124350329-12af8b00-dc2f-11eb-8a14-6131d12f4af4.png)

Choose File - CSR 파일을 업로드 해 준다.

![image](https://user-images.githubusercontent.com/28949235/124350339-2529c480-dc2f-11eb-9422-540b5f48b5e1.png)

이 파일도 다운로드 !



### p12 파일 만들기

> 지금은 공동 계정을 사용하기 때문에, development 페이지에서 key를 추가할 수 없었다. (개수 제한)  
> ![image](https://user-images.githubusercontent.com/28949235/124350208-3d4d1400-dc2e-11eb-90ee-dce75a023110.png)
>
> 그래서 p12 방식을 사용해야 한다.

![image](https://user-images.githubusercontent.com/28949235/124351519-d5023080-dc35-11eb-9577-d3640d0404c7.png)

![image](https://user-images.githubusercontent.com/28949235/124351581-2d393280-dc36-11eb-898d-3b21b4c1c97f.png)

export 눌러준다

![image](https://user-images.githubusercontent.com/28949235/124351659-a173d600-dc36-11eb-8ffd-43b776c57f1b.png)



![image](https://user-images.githubusercontent.com/28949235/124351676-b2244c00-dc36-11eb-82eb-2567bae445c3.png)

비번도 쳐주면

![image](https://user-images.githubusercontent.com/28949235/124351697-d4b66500-dc36-11eb-9c63-f7c22068fcba.png)

짠! p12 파일이 생성됐다.

### 개인 키 내보내기

![image](https://user-images.githubusercontent.com/28949235/124351725-062f3080-dc37-11eb-8546-86956f3508d0.png)

개인키만 클릭해서 Export

![image](https://user-images.githubusercontent.com/28949235/124351737-19da9700-dc37-11eb-88d5-efd5454b243b.png)

.p12로 save! 얘도 비번 또 설정해주고 저장해준다.

### pem 파일 생성하기

![image](https://user-images.githubusercontent.com/28949235/124351776-47bfdb80-dc37-11eb-815d-d94fe9b9b57a.png)

두 .p12 파일이 있는 디렉토리로 이동해준다.

```bash
openssl pkcs12 -clcerts -nokeys -out apns-cert.pem -in apns-cert.p12
```

위 명령어로 apns-cert의 pem 파일을 먼저 만들어준다.

![image](https://user-images.githubusercontent.com/28949235/124351804-6de57b80-dc37-11eb-85aa-b24b13102f01.png)

비번 쳐주면 완료!

```bash
openssl pkcs12 -nocerts -out apns-key.pem -in apns-key.p12
```

그 다음엔 위 명령어로 apns-key의 pem파일을 만들어준다.

![image](https://user-images.githubusercontent.com/28949235/124351827-8fdefe00-dc37-11eb-8b99-636118b260a7.png)

입력하라는 대로 비번 여러 번 쳐주면 완료!

```bash
openssl rsa -in apns-key.pem -out apns-key-noenc.pem
```

이제 두 pem 파일을 위 명령어로 합쳐준다.

![image](https://user-images.githubusercontent.com/28949235/124351845-abe29f80-dc37-11eb-949d-a7e0ac90ce83.png)

굿

```bash
cat apns-cert.pem apns-key-noenc.pem > apns-journey-enc.pem
```

요렇게 (이름은 맘대로) 최종적으로 apns-journey-enc.pem 파일을 만들어준다.

![image](https://user-images.githubusercontent.com/28949235/124351871-d92f4d80-dc37-11eb-9d3d-59ae8fdebd07.png)

짜자잔! 이제 요 놈을 (필요 시)노티 서버에 올리면 된다.



> 출처 [개발자 소들이](https://babbab2.tistory.com/)

