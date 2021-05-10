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

### array

```c++
sizeof() // 배열 크기
```

### vector

```c++
v.push_back(' '); // 맨 뒤에 원소 삽입
v.erase(v.begin() + index); // 특정 인덱스의 원소 삭제
```

### \<algorithm>

```c++
sort(처음, 끝); // 기본 오름차순 정리
```

```c++
// 벡터 내림차순 정렬
sort(v.begin(), v.end(), greater<int>());
```