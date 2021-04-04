---
title: "알고리즘 동아리 14주차 골드 문제 풀이"
excerpt: "21/04/04 - BOJ 16282, 18244, 14725"
tags: ["ps", "boj", "algorithm"]
categories: ["ps"]
last_modified_at: "2021-04-04"
toc: true
---

# A번 - [Black Chain](https://www.acmicpc.net/problem/16282){: target="\_blank"} (16282) 

## 문제 설명 

하나당 1g인 고리가 n만큼 서로 연결되어 있다. 이 체인을 최소로 끊어서(answer) 1g부터 _n_ g까지 무게를 잴 수 있어야 한다. 

## 풀이 

1개를 끊으면 2 1 4 로 끊을 수 있다.  
2개를 끊으면 3 1 6 1 9 로 끊을 수 있다.  
 
이 원리는 무엇인가?  
1g부터 _a_ g만큼 모두 잴 수 있다면 만약 _b_ g의 부분 체인이 있다면 1부터 _(a+b)_ g만큼 모두 잴 수 있다.  
이 원리를 이용해 k만큼 끊을 때 가장 고리가 긴 길이를 구하는 것이다.  
  
다시 귀납적 추론을 하자면   
  
끊는 횟수: 최대 고리 길이  
1: 2 1 4  
2: 3 1 6 1 12  
3: 4 1 8 1 16 1 32  
.  
.  
.  
  
$$
\begin{aligned}
&func(k) = k(1이 k개 있으므로) + (k+1) * 2^0 + (k+1) * 2^1 + (k+1) * 2^2 ... (k+1) * 2^k \\
&func(k) = (k+1)-1 ... \\
&func(k) = (k+1)(1 + 1 + 2 + 4 + ... + 2^k) - 1 \\
&func(k) = (k+1)2^{k+1} - 1  
\end{aligned}
$$  
  
즉, $(k+1)2^{k+1} - 1$이 k번 자를 때 가장 최대 길이이다.

## 코드 

```c++
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;
ll n;

void Input();
void Solve();

int main()
{
	Input();
	Solve();
}

void Input()
{
	ios::sync_with_stdio(false);
	cin.tie(NULL);
	cin >> n;
}

void Solve()
{
	ll ans = 1LL; ll bound = 2LL * (1LL << 2LL) - 1LL;
	while (n > bound)
	{
		ans++;
		bound = (ans + 1) * (1LL << (ans + 1)) - 1;
	}

	cout << ans;
}
```

## 소감 

골드의 벽은 너무 높다. 백준을 푼지 3년이 되었지만 아직 풍월을 읊지는 못하는구나...  
ICPC의 수학 문제를 간접 체험해 보아서 좋았다.  
앞으로 PS시 수기로 푸는걸 고려중... 수학문제에서 느꼈다.  

# B번 - [변형 계단 수](https://www.acmicpc.net/problem/18244){: target="\_blank"} (18244) 

## 문제 설명 



## 풀이 

## 코드 

## 소감 

# C번 - [개미굴](https://www.acmicpc.net/problem/14725){: target="\_blank"} (18244) 

## 문제 설명 

## 풀이 

## 코드 

## 소감 
