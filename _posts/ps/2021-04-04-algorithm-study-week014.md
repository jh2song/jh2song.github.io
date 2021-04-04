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

<br>

# B번 - [변형 계단 수](https://www.acmicpc.net/problem/18244){: target="\_blank"} (18244) 

## 문제 설명 

n개의 길이의 숫자들 중  

- 인접한 모든 자리수의 차이가 1이 난다.

- 인접한 자릿수는 연속으로 3번 이상 증가하거나 감소할 수 없다.

인 모든 개수를 구하는 문제이다. (큰 수 이므로 module 해준다.)

## 풀이 

일반적인 점화식  

```
dp[n][m][k]:
n길이 변형계단수에서
m이 끝자리 수 일때
k 상태

k: 0 (연속 2번 증가)
k: 1 (연속 1번 증가)
k: 2 (연속 1번 감소)
k: 3 (연속 2번 감소)

dp[n][m][0] = dp[n-1][m-1][1]
dp[n][m][3] = dp[n-1][m+1][2]
dp[n][m][1] = dp[n-1][m-1][2] + dp[n-1][m-1][3]
dp[n][m][2] = dp[n-1][m+1][0] + dp[n-1][m+1][1]
```

단 m이 0이거나 9일때는 특별한 처리를 해야 한다.

## 코드 

```c++
#include <bits/stdc++.h>
using namespace std;

#define DIV 1000000007
typedef long long ll;

ll n;
ll dp[101][10][4];

void Init();
void Input();
void Solve();

int main()
{
	Init();
	Input();

	if (n == 1)
	{
		cout << 10 << "\n";
		return 0;
	}

	Solve();
}

void Init()
{
	for (ll i = 0; i <= 9; i++)
	{
		if (i != 0)
			dp[2][i][1] = 1;
		if (i != 9)
			dp[2][i][2] = 1;
	}
}

void Input()
{
	cin >> n;
}

void Solve()
{
	for (ll i = 3; i <= n; i++)
	{
		for (ll j = 0; j <= 9; j++)
		{
			/*
			k: 0 (연속 2번 증가)
			k: 1 (연속 1번 증가)
			k: 2 (연속 1번 감소)
			k: 3 (연속 2번 감소)
			*/

			// 끝자리가 0일때 이전 상태가 증가가 2번 될 수 없다.
			if (j == 0)
			{
				dp[i][j][3] = dp[i - 1][j + 1][2] % DIV;
				dp[i][j][2] = dp[i - 1][j + 1][1] % DIV;
			}
			// 끝자리가 9일때 이전 상태가 감소가 2번 될 수 없다.
			else if (j == 9)
			{
				dp[i][j][0] = dp[i - 1][j - 1][1] % DIV;
				dp[i][j][1] = dp[i - 1][j - 1][2] % DIV;
			}
			else if (j >= 1 && j <= 8)
			{
				dp[i][j][0] = dp[i - 1][j - 1][1] % DIV;
				dp[i][j][1] = (dp[i - 1][j - 1][2] % DIV + dp[i - 1][j - 1][3] % DIV) % DIV;
				dp[i][j][2] = (dp[i - 1][j + 1][0] % DIV + dp[i - 1][j + 1][1] % DIV) % DIV;
				dp[i][j][3] = dp[i - 1][j + 1][2] % DIV;
			}
		}
	}

	ll ans = 0;
	for (ll i = 0; i <= 9; i++)
	{
		for (ll j = 0; j <= 3; j++)
		{
			ans += dp[n][i][j] % DIV;
			ans = ans % DIV;
		}
	}

	cout << ans % DIV << '\n';
}
```

## 소감 

문제를 깊이 이해하고 푸는 습관을 기르자.

<br>

# C번 - [개미굴](https://www.acmicpc.net/problem/14725){: target="\_blank"} (18244) 

## 문제 설명 

트라이 구조를 만든 뒤, 부모 노드부터 리프 노드까지의 깊이와 리프 노드를 출력하는 문제이다.

## 풀이 

트라이 클래스를 만들자.  
멤버 변수로는 map 자료구조로 또다시 트라이를 참조하는 구조이다.  

```c++
class Trie {
public:
	map<string, Trie*> child;
	void Insert(Trie* _root, vector<string> v, int idx);
};
```

## 코드 

```c++
#include <bits/stdc++.h>
using namespace std;

class Trie {
public:
	map<string, Trie*> child;
	void Insert(Trie* _root, vector<string> v, int idx);
};

void Trie::Insert(Trie* _root, vector<string> v, int idx)
{
	if (idx == v.size()) return;

	if (_root->child.count(v[idx]) == 0)
		_root->child[v[idx]] = new Trie();

	Insert(_root->child[v[idx]], v, idx + 1);
}

void Input();
void Solve();

int t;
int n;
Trie* root = new Trie();

void DFS(Trie* _root, int dep = 0)
{
	for (auto e : _root->child)
	{
		for (int i = 0; i < dep; i++)
			cout << "--";
		cout << e.first << "\n";
		DFS(e.second, dep + 1);
	}
}

int main()
{
	Input();
	Solve();
	return 0;
}

void Input()
{
	cin >> t;

	while (t--)
	{
		cin >> n;
		vector<string> v(n);
		for (int i = 0; i < n; i++)
			cin >> v[i];

		root->Insert(root, v, 0);
	}
}

void Solve()
{
	DFS(root);
}
```

## 소감 

참조를 하면 풀 수 있지만 실제 코테나 대회에선 참조를 할 수 없진 않는가? 시간이 나면 틈틈히 문제 복기를 해야겠다.