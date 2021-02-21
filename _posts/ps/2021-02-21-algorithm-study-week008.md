---
title: "알고리즘 동아리 8주차 골드 문제 풀이"
excerpt: "21/02/21 - BOJ 2533, 14698, 2616"
tags: ["ps", "boj", "algorithm"]
categories: ["ps"]
last_modified_at: "2021-02-21"
toc: true
toc_sticky: true
---

# A번 - [사회망 서비스(SNS)](https://www.acmicpc.net/problem/2533){: target="\_blank"} (2533)

## 문제 설명

트리가 주어진다. 어떤 노드의 주위가 모두 얼리어답터이면 그 노드는 새로운 아이디어를 받아들인다. 얼리어답터를 최소로 해서 모든 노드가 새로운 아이디어를 받아들이게 하는 경우를 구하는 문제이다.

## 풀이

- 점화식

  > dp[i][j]: 부모가 i인 서브트리의 얼리 어답터의 최대값  
  > (j가 0이면 i 노드가 얼리어답터, j가 1이면 i 노드가 얼리어답터X)

- 수도 코드

```c++
dp[i][0] = dp[i->child1][1] + dp[i->child2][1] + ...
dp[i][1] = min(dp[i->child1][1], dp[i->child1][0])
    + min(dp[i->child2][1], dp[i->child2][0])
    + ...
```

## 코드

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <cstring>
using namespace std;

vector<int> adj[1000001];
vector<int> children[1000001];
bool visited[1000001];

// j가 0이면 i번째 노드가 얼리어답터가 아닌 서브 트리의 얼리어답터 최소 수
// j가 1이면 i번째 노드가 얼리어답터인 서브 트리의 얼리어답터 최소 수
int dp[1000001][2];

int n;
int a, b;

void makeChildren(int root);
int solve(int i, int j);

int main()
{
	ios::sync_with_stdio(false);
	cin.tie(NULL);

	memset(dp, -1, sizeof(dp));

	cin >> n;
	for (int i = 0; i < n - 1; i++)
	{
		cin >> a >> b;
		adj[a].push_back(b);
		adj[b].push_back(a);
	}

	visited[1] = true;
	makeChildren(1);

	dp[1][0] = solve(1, 0);
	dp[1][1] = solve(1, 1);

	cout << min(dp[1][0], dp[1][1]);
	return 0;
}

void makeChildren(int root)
{
	vector<int> rootChildren;

	for (int adjNode : adj[root])
	{
		if (visited[adjNode])
			continue;

		children[root].push_back(adjNode);
		visited[adjNode] = true;
		rootChildren.push_back(adjNode);
	}

	for (int child : rootChildren)
	{
		makeChildren(child);
	}
}

int solve(int i, int j)
{
	int& ret = dp[i][j];
	if (ret != -1)
		return ret;

	if (j == 0)
		ret = 0;
	else if (j == 1)
		ret = 1;

	// relChildren: relevant children (관련된 자식들)
	vector<int> relChildren = children[i];

	if (j == 0)
	{
		for (int child : relChildren)
		{
			ret += solve(child, 1);
		}
	}
	else if (j == 1)
	{
		for (int child : relChildren)
		{
			ret += min(solve(child, 0), solve(child, 1));
		}
	}

	return ret;
}
```

## 소감

매우 interesting한 문제이다. DP 꿀잼 ㅋㅋㅋ 트리 DP 문제는 그래프를 트리로 바꾸는 테크닉이 필요한데 코드의 `makeChildren` 함수를 잘 기억하자.

<br>

# B번 - [전생했더니 슬라임 연구자였던 건에 대하여 (Hard)](https://www.acmicpc.net/problem/14698){: target="\_blank"} (14698)

## 문제 설명

슬라임이 여러 마리 있다. 슬라임을 적절히 합성하여 1마리의 슬라임으로 만드려 한다. 두 슬라임 에너지의 곱의 크기만큼 전기 에너지가 소요되고 그 크기가 새로운 슬라임의 에너지가 된다. 합성 단계마다 필요한 전기 에너지를 모두 곱한 값이 최소가 되도록 합성을 해야 한다. 슬라임을 끝까지 합성했을 때 청구될 비용의 최소값을 1,000,000,007로 나눈 나머지를 출력해야 한다.

## 풀이

작은것부터 곱해 나가면 된다. moduler 연산이 매우매우 중요한데 작은거 2개를 곱한 뒤 `정답 = 정답 * (곱한 값 % DIV) % DIV` 으로 계산해야 정확하게 된다. 안 그러면 Overflow가 날 가능성이 크다.

## 코드

```c++
#include <iostream>
#include <queue>
#include <functional>

#define DIV 1000000007

using namespace std;

typedef long long ll;

int main()
{
	ios::sync_with_stdio(false);
	cin.tie(NULL);

	int t;
	cin >> t;
	while (t--)
	{
		priority_queue<ll, vector<ll>, greater<ll>> pq;

		int n;
		cin >> n;

		ll d;
		while (n--)
		{
			cin >> d;
			pq.push(d);
		}

		ll answer = 1;
		while (pq.size() != 1)
		{
			ll p1 = pq.top();
			pq.pop();
			ll p2 = pq.top();
			pq.pop();
			ll newEle = p1 * p2;
			answer = answer * (newEle % DIV) % DIV;
			pq.push(newEle);
		}

		cout << answer << "\n";
	}
}
```

## 소감

작은것부터 곱해나가면 된다는것을 빠르게 catch 하지 못하였다. 사실 이 문제는 실버급 문제라고 생각한다. 우선순위 큐만 쓰면 되기 때문이다.

<br>

# C번 - [소형기관차](https://www.acmicpc.net/problem/2616){: target="\_blank"} (2616)

## 문제 설명

## 풀이

## 코드

## 소감
