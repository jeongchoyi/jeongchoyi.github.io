---
layout: post
title: "Dynamic Programming" 
date: 2021-06-06
category: read 
excerpt: ""

---

# Dynamic Programming (DP)

> 큰 문제를 **단위 문제**로 나누어서 풀 때,  
> **단위 문제를 풀기 위한 연산이 반복되는 경우**에,  
> 불필요한 반복을 줄이기 위해 단위 문제의 연산 결과를 **메모리에 기록해두고 재활용**하는 방식

## 맛보기

### 피보나치 수 구하기

피보나치 수열 : 다음 항이 이전 항 두개의 합으로 표현되는 수열  

```
fibo(n) = fibo(n-1) + fibo(n-2)
```

#### 1. Bruteforce로 풀기

> 위의 식을 재귀함수로 바꿔서 그대로 푸는 법. ~~컴퓨터야 미안해~!!~~

```c++
int fibo(n) {
  if(n==1 || n==0){
    return 1
  } else {
    return fibo(n-1)+fibo(n-2)
  }
}
```

이렇게 했을 때, 만약 **fibo(5)**를 호출하면 `fibo(2)`는 몇 번 호출될까?  
fibo(4) + fibo(3) = fibo(3) + fibo(2) + fibo(2) + fibo(1) = fibo(2) + fibo(1) + fibo(2) + fibo(2) + fibo(1)   
총 3번 !

**fibo(10)** 을 호출하면 `fibo(2)` 는 34번, **fibo(15)** 는 `fibo(2)` 는 377번 호출된다 ㄷㄷ...  
이렇게 많이 호출되는 건 `fibo(2)` 뿐만이 아닐텐데... ~!! ㅠ___ㅠ

#### 2. Dynamic Programming으로 풀기

위 상황처럼,  
**단위 문제를 풀기 위한 연산이 반복디는 경우에 그 반복을 줄여주는 해결책**으로 등장한 DP! 





1. 메모리를 선언한다
2. 단위 문제에서 처음 연산이 일어나면 메모리에 저장한다
3. 그 다음부터는 메모리에 있는 값들을 참조하여 사용한다

```c++
int mem[100] = {0, } // 메모리 선언하기

int fibo(n){
  if ( mem[n]==0 && n!=0) { // 처음 연산이 일어나면
    mem[n] == fibo(n-1) + fibo (n-2) // 메모리에 저장하고
  }
  return mem[n] // 메모리에 있는 값을 참조해서 사용 (처음 연산이 아닐 경우 바로 반환)
}
```

![image](https://user-images.githubusercontent.com/28949235/120914163-1f47ce80-c6d7-11eb-90b3-129c54aa6eab.png)

X 표시 한 것들은 메모리에 있으므로 계산할 필요가 없어지는 것 !

위와 같은 방식을 **메모이제이션** 이라고 부르는데,  
Bruteforce를 사용할 경우 (**메모이제이션 사용 X**)의 시간 복잡도는 **2^n**,  
DP를 사용할 경우 (**메모이제이션 사용 O**)의 시간 복잡도는 **n**

## LCS (Longest Common Subsequence) 구하기

> 동적 계획법에서 굉장히 유명한 문제 !

> * Longest Common Substring ( 최장 공통 문자열 )
>
>   * vocabulary와 february에서
>
>   * 공통 부분 문자열 : voca**b**ul**ary** fe**b**ru**ary**
>
>     -> "b", "ary"
>
>   * 최장 공통 부분 문자열 : vocabul**ary** febru**ary**
>
>     -> "ary"
>
> * Longest Common Subsequence ( 최장 공통 부분수열 )
>
>   * **연속성 고려 X 순서 고려 O**
>
>   * vocabulary와 february에서
>
>   * 최장 공통 부분수열 : voca**bu**l**ary** fe**b**r**uary**
>
>     -> buary

#### 1. Bruteforce로 풀기

1. 첫번째 문자열의 모든 부분 수열의 집합을 구한다.
2. 두번째 문자열의 모든 부분 수열의 집합을 구한다
3. 부분 수열 집합 두 개를 비교하여 같은 것을 모두 찾아낸 후 (교집합 연산),  
   가장 긴 부분 수열을 출력한다.

> 문자열 길이가 n일 때, 해당 문자열의 부분수열의 개수는 2^n개

#### 2. Dynamic Programming으로 풀기

> 단위문제로 나눠보자. 편의상 뒤에서부터 비교했고, 앞에서부터 해도 상관 없음

주어진 문자열 둘의 길이를 m, n 이라고 할 때  
각 문자열을 배열로 표현하면 `X[0 .. m-1]` `Y[0 .. n-1]`

`L(X[0..m-1], Y[0..n-1])` 을 두 문자열 X, Y의 LCS 길이를 반환하는 함수라고 할 때,



1. 두 문자열의 마지막 문자가 같다면 (`X[m-1] == Y[n-1]`)

   `L(X[0..m-1], Y[0..n-1]) = 1 + L(X[0..m-2], Y[0..n-2])`

   즉, 문자열 **맨 뒤**에서 같은 **문자를 찾으면**  
   LCS의 길이를 1 증가시키고, 각 문자열을 하나씩 줄인 뒤, 다시 LCS를 찾는 !! 재귀형식 접근인 것

   **pororo와 tomato의 마지막 문자 'o'가 같으므로,**

   **LCS('pororo', 'tomato') = LCS('poror', 'tomat') + 1** 



2. 두 문자열의 마지막 문자가 같지 않다면 ( `X[m-1] != Y[n-1]` )

   `L(X[0..m-1], Y[0..n-1]) = MAX(L(X[0..m-2], Y[0..n-1]), L(X[0..m-1], Y[0..n-2]))`

   즉, 문자열 **맨 뒤**에서 같은 **문자를 찾지 못하면**  
   두 문자열 중 하나만 다음 LCS로 넘어가고 하나는 그대로 냅둠

   **poror와 tomat의 마지막 문자가 같지 않으므로,**

   **LCS('poror', 'tomat') = MAX(LCS('poro', 'tomat'), LCS('poror', 'toma'))**



이러한 재귀호출을 반복하다 보면 특정 문자열들을 비교하는 함수가 반복 호출된다!

![image](https://user-images.githubusercontent.com/28949235/120916912-6938b080-c6e7-11eb-9be1-4d3d5ef3c3aa.png)

이 표를 2차원 배열로 만들어서 채워나가는 식으로 풀면 된다.

```python
# DP 를 적용한 LCS 문제 솔루션 
def lcs(X , Y): # 주어진 문자열의 길이를 구함. 
  m = len(X) 
  n = len(Y) 
  # DP 값을 저장하기 위한 배열을 선언 
  L = [[None]*(n+1) for i in xrange(m+1)] 
  """L[m+1][n+1] 를 bottom up 방식으로 채워나갑니다. 참고: L[i][j] 은 X[0..i-1] 와 Y[0..j-1] 의 LCS 길이를 가지고 있습니다.""" 
  for i in range(m+1): 
    for j in range(n+1): 
      if i == 0 or j == 0 : 
        L[i][j] = 0 
      elif X[i-1] == Y[j-1]: 
        L[i][j] = L[i-1][j-1]+1 
      else: 
        L[i][j] = max(L[i-1][j] , L[i][j-1]) 
        # L[m][n] 은 X[0..n-1] & Y[0..m-1] 의 LCS 의 길이가 저장되어 있습니다. 
        return L[m][n] 
      #LCS 함수의 마지막 
      # 위의 알고리즘을 테스트 하기 위한 드라이버 프로그램 
      X = "AGGTAB" 
      Y = "GXTXAYB" 
      print "Length of LCS is ", lcs(X, Y) 
      # 이 코드는 Nikhil Kumar Singh(nickzuck_007) 에 의해 작성되었습니다.

출처: https://zzonglove.tistory.com/26 [난뭐야]
```



## 정리하기

큰 문제를 **단위 문제**로 나누어서 풀 때,  
단위 문제를 풀기 위한 연산이 반복되는 경우에,  
불필요한 반복을 줄이기 위해 단위 문제의 연산 결과를 메모리에 기록해두고 재활용하는 방식



즉, 다이나믹 프로그래밍은 **하나의 문제를 한 번만 풀도록**하는 알고리즘 ! (-동빈나)  
다만, 그 문제는 어느 타이밍에 답을 얻었느냐에 따라 답이 달라지는 문제가 아니어야 함.



**단위 문제로 나누어 풀 수 있는 문제에 대하여  **
**단위 문제 해결 동작의 반복을 줄이기 위하여  **
**메모이제이션을 사용하는 알고리즘**



단순하지만 매우 큰 호율을 부를 수 있는 훌륭한 이념 하나로  
정말 다양한 문제를 풀 수 있기에 자주 출제됨

