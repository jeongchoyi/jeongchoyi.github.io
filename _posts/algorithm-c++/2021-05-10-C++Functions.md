---
layout: post
title: "C++ Functions" 
date: 2021-05-10
category: read 
excerpt: ""


---

# C++ 함수 아카이브

> 알고리즘 풀이에 자주 쓰이는 함수 아카이브

### 입력 시간 줄이기

```c++
 cin.tie(NULL); cout.tie(NULL);
 ios_base::sync_with_stdio(false); //main 함수 내에 작성
```

### for

```c++
for(int i:vectorName) {...} // i가 vectorName 내 원소들을 순회
```

### array

```c++
sizeof() // 배열 크기
```

### vector

```c++
v.push_back(' '); // 맨 뒤에 원소 삽입
v.erase(v.begin() + index); // 특정 인덱스의 원소 삭제
```

```c++
vector<int> students(n+2, 0) //n+2의 size만큼 0으로 초기화 (선언 시)
```

```c++
 answer.insert(answer.end(), temp.begin(), temp.end()); //answer벡터에 temp벡터 붙이기
```



### \<algorithm>

```c++
sort(처음, 끝); // 기본 오름차순 정리
```

```c++
// 벡터 내림차순 정렬
sort(v.begin(), v.end(), greater<int>());
```

```c++
// 배열 내 중복값 제거
arr.erase(unique(arr.begin(), arr.end()),arr.end());
```

```c++
// string벡터 사전순 정렬
bool comp(string s1, string s2){
  return s1 < s2; //사전순 정렬
}
// ...
string arr[5] = {"asda", ..., "asdss"};
sort(arr, arr+5, comp)
```

```c++
// 대칭 차집합 연산 (겹치는거 다 빼고 남은것만 반환하는게 대칭차집합)
// 두 벡터 다 정렬되어있어야 하는 듯?
auto iter = set_symmetric_difference(v1.begin(), v1.end(), v2.begin(), v2.end(), result.begin());
```



### \<string>

```c++
string str1 = to_string(12345);
str1.at(index); // char 반환
str1.size();
str1.length(); // 길 (size함수와 같음)
```

```c++
str1.append("d");  // char는 append 못함.
str1 += "d" // 위랑 같음
str2.append(str1, 0, 4); // str1의 0~4 idx만큼 str2 뒤에 append
```

```c++
stoi(str1); // int로
stof(str1); // float
stol(str1); // long
stod(str1); // double
stoll(str1); // long long
```

```c++
str1.erase(str1.begin() + idx);
str1.push_back(s); // char를 append할 때만 사용 가능
```

```c++
s[i] = toupper(s[i]); //대문자로
s[i] = tolower(s[i]); //소문자로
isupper(s[i]); //대문자인지
islower(s[i]); //소문자인지
```

```c++
s.substr(시작점 인덱스,캐릭터 개수);
```



