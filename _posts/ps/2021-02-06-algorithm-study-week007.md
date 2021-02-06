---
title: "알고리즘 동아리 7주차 골드 문제 풀이"
excerpt: "21/02/06 - BOJ 1759, 12865, 3055"
tags: ["ps", "boj", "algorithm"]
categories: ["ps"]
last_modified_at: "2021-02-06"
toc: true
toc_sticky: true
---
# A번 - [암호 만들기](https://www.acmicpc.net/problem/1759){: target="_blank"} (1759)

## 문제 설명

일반적인 N과 M 문제와 유사하다. C개의 문자를 L개씩 뽑아 나열한다.(순열 개념) 그때 뽑은 문자열은

1. 오름차순 정렬
2. 모음 최소 1개
3. 자음 최소 2개

의 조건을 만족해야 한다. 그 문자열을 순서대로 출력해야 한다.

## 풀이

1. C개의 문자 벡터를 오름차순으로 정렬
2. idx 0 부터 백트레킹해서 선택된 요소들을 담는 벡터에 순차적으로 담는다.
3. 모두 선택 되었으면 위의 조건을 검사
4. 조건이 성립되면 출력, 조건이 거짓이면 return

## 코드

|메모리|시간|
|---|---|
|2016 KB|0 ms|

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int l, c;
vector<char> v;
vector<char> sv;

void Back(int idx)
{
	if (sv.size() == l)
	{
		int vow = 0;
		int con = 0;
		for (auto& e : sv)
		{
			if (e == 'a' || e == 'e' || e == 'i' || e == 'o' || e == 'u')
				vow++;
			else
				con++;
		}

		if (vow >= 1 && con >= 2)
		{
			for (auto& e : sv)
			{
				cout << e;
			}
			cout << "\n";
		}
		else
			return;
	}

	for (int i = idx; i < (int)v.size(); i++)
	{
		sv.push_back(v[i]);
		Back(i + 1);
		sv.pop_back();
	}

	return;
}

int main()
{
	cin >> l >> c;
	
	char d;
	for (int i = 0; i < c; i++)
	{
		cin >> d;
		v.push_back(d);
	}
	sort(v.begin(), v.end());

	Back(0);
	return 0;
}
```

## 소감

아예 백트레킹을 하면서 모음과 자음을 체크해서 선택할 수 없을까 하는 의문이 남는다.

<br>

# B번 - [평범한 배낭](https://www.acmicpc.net/problem/12865){: target="_blank"} (12865)

## 문제 설명

무게와 가치가 차례대로 주어진다. 준서가 버틸 수 있는 무게(K) 선에서 가장 가치가 높게 고를때의 합을 구하는 문제다.

## 풀이

dp[i][j]: (i번째) 짐까지 탐색시 (j무게)에서 나올 수 있는 최대 가치

* 만약 (j무게)가 (i번째) 짐의 무게보다 작을 시
> dp[i][j] = dp[i - 1][j];
* 그렇지 않다면
> dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight] + value);
* 최종 정답 (n번째 짐까지 탐색시 k무게에서 나올 수 있는 최대 가치)
> dp[n][k];

## 코드

|메모리|시간|
|---|---|
|41472 KB|32 ms|

```c++
#include <iostream>
#include <algorithm>
using namespace std;

int n;
int k;
int arr[101][2];
int dp[101][100001];

int main()
{
	ios::sync_with_stdio(false);
	cin.tie(NULL);

	cin >> n >> k;
	for (int i = 1; i <= n; i++)
	{
		cin >> arr[i][0] >> arr[i][1];
	}

	// dp[i][j]: (i번째) 짐까지 탐색시 (j무게)에서 나올 수 있는 최대 가치
	for (int i = 1; i <= n; i++)
	{
		int weight = arr[i][0];
		int value = arr[i][1];
		for (int j = 1; j <= k; j++)
		{
			if (j < weight)
				dp[i][j] = dp[i - 1][j];
			else
				dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight] + value);
		}
	}

	cout << dp[n][k];
}
```

## 소감

DP는 어렵다... 점화식 세우기가 까다롭다. 실버 문제를 짬짬히 풀면서 DP 감을 다져놔야겠다.

<br>

# C번 - [탈출](https://www.acmicpc.net/problem/3055){: target="_blank"} (3055)

## 문제 설명

배열은 크게 3가지로 구성된다. 'D', 'S', '.', '*', 'X'

<br>

* 'D' : 최종 도착지
* 'S' : 시작점
* '.' : 비어있는 점
* '\*' : 물의 좌표. 물은 매 분 4방향으로 '.' 을 '*'로 채운다.
* 'X' : 벽
<br>

S에서 시작해서 D까지 무사히 갈 수 있으면 탐색 비용을 출력하고 그렇지 않다면 "KAKTUS"을 출력해야 한다.

## 풀이

BFS를 두 번 해야 한다. 물의 상태를 먼저 탐색해야 할까? 아니면 고슴도치의 상태를 먼저 탐색해야 할까? 정답은 물의 상태를 먼저 탐색해야 한다. 

> 고슴도치는 물이 찰 예정인 칸으로 이동할 수 없다.

BFS를 하면서 고슴도치의 상태가 최종 도착지 'D'점에 도착하면 탐색 비용을 출력하고 BFS시 해당 점에 갈 수 없다면 "KAKTUS"를 출력한다.

## 코드

|메모리|시간|
|---|---|
|2152 KB|0 ms|

```c++
#include <iostream>
#include <queue>
using namespace std;

typedef pair<int, int> pii;

int n, m;
char arr[51][51];
bool visited[51][51];
int endY, endX;
int dy[4] = { -1,0,1,0 };
int dx[4] = { 0,1,0,-1 };
queue<pii> q, q2;

int BFS();

int main()
{
	ios::sync_with_stdio(false);
	cin.tie(NULL);

	cin >> n >> m;
	for (int i = 1; i <= n; i++)
	{
		for (int j = 1; j <= m; j++)
		{
			cin >> arr[i][j];
			if (arr[i][j] == 'D') // 비버의 굴이면
			{
				endY = i;
				endX = j;
			}
			else if (arr[i][j] == 'S') // 고슴도치 시작 위치면
			{
                q2.push({ i, j });
			}
            else if (arr[i][j] == '*')
            {
                q.push({ i,j });
            }
		}
	}

	int ans = BFS();
    if (ans == -1)
        cout << "KAKTUS";
    else
        cout << ans;

	return 0;
}

int BFS()
{
    int ret = 0;
    while (true) 
	{
        // 물
        int qSize = q.size();
        for (int i = 0; i < qSize; i++) 
		{
            int y = q.front().first;
            int x = q.front().second;
            q.pop();
            if (visited[y][x] == true)
                continue;
            visited[y][x] = true;

            for (int j = 0; j < 4; j++) 
			{
                int nextY = y + dy[j];
                int nextX = x + dx[j];

                // 범위 바깥이거나 X이거나 방문했으면
                if (nextY < 1 || nextY > n || nextX < 1 || nextX > m || arr[nextY][nextX] == 'X' ||
                    visited[nextY][nextX] == true || arr[nextY][nextX] == 'D')
                    continue;

                q.push({ nextY, nextX });
            }
        }

        // 고슴도치
        int q2Size = q2.size();
        for (int i = 0; i < q2Size; i++) 
		{
            int y = q2.front().first;
            int x = q2.front().second;
            q2.pop();
            if (visited[y][x] == true)
                continue;
            visited[y][x] = true;

            // y, x가 D의 좌표로 왔으면
            if (y == endY && x == endX)
                return ret;

            for (int j = 0; j < 4; j++) 
			{
                int nextY = y + dy[j];
                int nextX = x + dx[j];

                // 범위 바깥이거나 X이거나 방문했으면
                if (nextY < 1 || nextY > n || nextX < 1 || nextX > m || arr[nextY][nextX] == 'X' ||
                    visited[nextY][nextX] == true)
                    continue;

                q2.push({ nextY, nextX });
            }
        }

        if (q.empty() && q2.empty())
            break;

        ret++;
    }

    return -1;
}
```

## 소감

BFS를 두 번 해야 된다는 신박한 문제다. 매우 좋은 문제인 듯 하다. BFS/DFS 문제는 풀 때는 매우 귀찮다. ㅋㅋㅋ 구현이 매우 손이 많이 가기 때문이다. 그래도 귀찮은걸 참고 첫 트에 AC를 맞아서 기뻤다. 아마 귀찮아 하는 것도 아직 BFS/DFS가 아직 눈 감고 할 정도로 손에 안 익어 적응이 안되서 그런 것일 수도 있다.