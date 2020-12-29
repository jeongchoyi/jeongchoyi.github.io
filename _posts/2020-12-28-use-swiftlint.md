---
layout: post
title: "SwiftLint 사용하기" 
date: 2020-12-28
category: read 
excerpt: ""

---

# SwiftLint 사용하기

초기 프로젝트 설정 해서 올리면서 pod 몇개 깔았는데.. 그 중에 프로젝트 일관성을 위한,, SwiftLint

```
  # Pods for MoMo
	pod 'SwiftLint'
	pod 'Alamofire', '~> 5.2'
	pod 'Moya', '~> 14.0'
```

> Moya는 .. 이번에 공부하고 싶어서 설치해놨는데 과연 ㅎㅎ ㅋ

암튼! 설치한다고 끝이 아니라 SwiftLint를 사용하려면 몇가지 설정을 해 줘야 한다.

![image](https://user-images.githubusercontent.com/28949235/103266195-64622900-49f2-11eb-95f3-036db50246bf.png)

TARGETS - Build Phases - New Run Script Phase를 추가 해 준다.

![image](https://user-images.githubusercontent.com/28949235/103266202-6c21cd80-49f2-11eb-855a-3f92819057ba.png)

```
"${PODS_ROOT}/SwiftLint/swiftlint"
```

라고 써준다. (Based on dependency analysis는 체크 해제 했음)

### .swiftlint.yml 파일 만들기

우선 처음에는 오류를 말 그대로 뿜는다.. 나 프로젝트 만들고 pods 설치하고 폴더링 밖에 안 했는데..
![image](https://user-images.githubusercontent.com/28949235/103266389-d33f8200-49f2-11eb-84c9-716590e78a39.png)

374개랑 30개 ㅋㅋ ㅋㅋ ;;

![image](https://user-images.githubusercontent.com/28949235/103266417-e0f50780-49f2-11eb-98ec-09d53f211312.png)

프로젝트 폴더 내부 말고! 프로젝트.. 모라고 하냐 암튼 저기에 .swiftlint.yml 파일을 생성해준 후,

```
disabled_rules:
- line_length
- trailing_whitespace
- orphaned_doc_comment
- nesting
included:
excluded:
- Pods
- MoMo/Sources/AppDelegate.swift
- MoMo/Sources/SceneDelegate.swift
```

이렇게 써준다.

`disabled_rules` - 특정 rule을 무시하는 것.

`included` - 특정 파일 포함

`excluded` - 특정 파일 제외 
	(Pods..내가 만든 거 아닌데 나한테 오류 뿜으면 우짤?, AppDelegate랑 SceneDelegate 파일)

![image](https://user-images.githubusercontent.com/28949235/103266586-42b57180-49f3-11eb-81e0-21145db94efc.png)

깔 끔 ~~

근데 아마 나중에 강도 제약 살짝 낮춰야 할 것 같기도 ...



나중에 참고할 것 같은 곳  
[여기](https://gwangyonglee.tistory.com/44)