---
layout: post
title: "Bruteforce & Backtracking" 
date: 2021-05-02
category: read 
excerpt: ""

---

## Bruteforce

> 문제를 해결하기 위해 가능한 모든 경우를 직접 구하는 것  
> Bruteforce 알고리즘으로 문제를 풀면 무조건 정답이 나올 수 밖에 없다. → 노가다

> 브루트포스의 가장 큰 단점:  
> 숫자가 커지면 커질수록 그만큼 시간 복잡도가 엄청나게 커진다  
> 따라서 입력 데이터의 크기가 작고, 내가 구현할 시간복잡도 내에 해결 가능할 때만 사용해야 한다. 

### 풀이

보통 재귀(top-down) / 반복문(bottom-up) 사용\
**재귀(top-down)**

```c++
int recurAdd(int n){
		if(n==1) return 1;
		return n + recurAdd(n-1);
}
```

**반복문(bottom-up)**

```c++
int sum = 0;
for(int i=1; i<=100; i++){
	sum += i;
}
```

### 문제 유형

1. 순열 만들기 ⇒ O(N!)
   - n 제한을 꼭 확인해야 함. n이 11만 넘어가도 1초를 넘김
2. 조합 만들기
3. 2^n 경우의 수 만들기
   - 모든 질문에 yes/no 로 나눠질 때 전체 2^n가지

## Backtracking

> 모든 경우의 수를 탐색하되,  
> 문제가 요구하는 어떠한 제약 조건을 만족하지 않는 경우는 더이상 탐색하지 않고 종료하는 것

> 어떤 노드의 유망성을 점검후, 유망하지 않으면 부모노드로 돌아가 다른 자식 노드를 검사한다.

### State Space Tree (상태 공간 트리)

> 백트래킹에서 고려해야할 경우의 수를 트리로 나타낸 것

![image](https://user-images.githubusercontent.com/28949235/116803857-dc359100-ab55-11eb-9b10-97723ea97f45.png)

상태공간 트리에서 점진적으로 자식 노드로 향하면서 유망성(제약 조건에 맞는지)을 검사하고,  
만약 유망성이 없다면 더 이상 탐색하지 않고 다시 부모노드로 돌아간다 ⇒ **Backtracking**

### 풀이

주로 재귀 이용

```python
def backtracking(current node) {
	if  현재 해답에 도달했다면 {
		print(current 상태)
		return
	}	
	if(유망성검사 함수(current node)){
		for(자식노드 child) {
			backtracking(child) //재귀
		}
	}
	else {
		return   //유망하지 않으면 바로 종료하고 돌아감
	}
}
```

### 문제 유형

완전 탐색으로 풀 수 있는 입력 제한이 작은 문제 + 가능한 경우의 수들이 모두 어떤 제약 조건을 통과할 때

대표적인 문제들 → 스도쿠, N-Queens, 0-1 knapsack problem 등

