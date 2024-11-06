## 탐색

> 탐색은 주어진 데이터에서 자신이 원하는 데이터를 찾아내는 알고리즘을 말한다. 주어진 데이터의 성질(정렬 비정렬)에 따라 적절한 탐색 알고리즘을 선택하는 것이 중요하고, 실제 모든 코딩 테스트 문제의 기본이 되는 알고리즘이므로 제대로 이해하는 것이 중요하다.

### 깊이 우선 탐색(DFS)

깊이 우선 탐색은 그래프 완전 탐색 기법 중 하나이다. 깊이 우선 탐색은 그래프의 시작 노드에서 출발하여 탐색할 한쪽 분기를 정하여 최대 깊이까지 탐색을 마친 후 다른 쪽 분기를 이동하여 다시 탐색을 수행하는 알고리즘이다. *스택이나 재귀함수*를 이용하여 구현할 수 있다.

해당 문제의 유형으로는 **단절점 찾기**, **단절선 찾기**, **사이클 찾기**, **위상 정렬** 등이 있다.

#### 깊이 우선 탐색의 핵심 이론

DFS는 한 번 방문한 노드를 다시 방문하면 안 되므로 노드 방문 여부를 체크할 배열이 필요하며, 그래프는 인접 리스트로 표현한다. DFS탐색은 재귀함수를 이용하여 구현할 수 있으며, 스택을 이용하여 구현할 수도 있다.

#### 연결 요소의 개수 구하기

```cpp
#include <iostream>
#include <vector>
using namespace std;

static vector<vector<int>> A;
static vector<bool> visited;
void DFS(int v);

int main()
{
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);

	int N, M;
	cin >> N >> M;
	A.resize(N + 1);
	visited = vector<bool>(N + 1, false);

	for (int i = 0; i < M; i++)
	{
		int u, v;
		cin >> u >> v;
		A[u].push_back(v);
		A[v].push_back(u);
	}

	int count = 0;

	for (int i = 1; i <= N; i++)
	{
		if (!visited[i])
		{
			DFS(i);
			count++;
		}
	}

	cout << count << '\n';
}

void DFS(int v)
{
	visited[v] = true;

	for (int i : A[v])
	{
		if (!visited[i])
			DFS(i);
	}
}
```

