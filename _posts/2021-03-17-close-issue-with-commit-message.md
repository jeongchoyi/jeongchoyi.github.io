---
layout: post
title: "커밋 메세지로 이슈 닫기" 
date: 2021-03-17
category: read 
excerpt: ""

---

# 커밋 메세지로 이슈 닫기

**"`Fixes` 라는 키워드로 이슈를 닫을 수 있다더라!"**  
라는 말만 듣고 MOMO를 진행하면서 열심히 `Fixes`를 썼는데...  
이슈에 멘션만 될 뿐 이슈가 닫히지 않았었다.

![image](https://user-images.githubusercontent.com/28949235/111635291-b3be3580-883a-11eb-85bd-6cd931bfb5fa.png)

> 이렇게 썼었는데...

PR을 날리면서 Related Issues에 Fixes라는 키워드를 사용했고,  
![image](https://user-images.githubusercontent.com/28949235/111635384-c89ac900-883a-11eb-9e27-e7762c7e0218.png)

이렇게 뜨는데 452번 이슈에서는

![image](https://user-images.githubusercontent.com/28949235/111635469-d8b2a880-883a-11eb-8616-ad1d1f46cba1.png)

이렇게만 뜨고 이슈는 일일이 닫아줬어야 했다.  
팀원들도 다 같이 어리둥절...했었는데!

이제서야 이유를 알았다 ㅎ  
PR에 쓰는 게 아니고 커밋 메세지에 쓰는 거였다,.,,,,,,,!!!!!

[Github Docs](https://docs.github.com/en/github/managing-your-work-on-github/linking-a-pull-request-to-an-issue)에 보면

```
close
closes
closed
fix
fixes
fixed
resolve
resolves
resolved
```

위와 같은 키워드를 커밋 메세지에 작성하면 이슈를 자동으로 닫아준다고 한다.  
이슈가 PR과 같은 repo에 있냐 없냐에 따라서 사용법이 조금 다른데, 아래와 같다!
![image](https://user-images.githubusercontent.com/28949235/111635713-216a6180-883b-11eb-935a-41c04a0d57b1.png)



프로젝트 레포 컨벤션 고쳐야겠다...~

### 추가

이슈가 머지될 브랜치가 default branch여야 자동으로 이슈가 닫힌다..!!

> You can link a pull request to an issue by using a supported keyword in the pull request's description or in a commit message (**please note that the pull request must be on the default branch**).

