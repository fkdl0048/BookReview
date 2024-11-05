## 정렬

*이미 정리한 내용이 있어서 가볍게 정리*

- [정렬 알고리즘 정리 CodeReview](https://github.com/fkdl0048/CodeReview/blob/main/Algorithm/Sort/README.md)

| 정렬 알고리즘 | 정의 |
|:---|:---:|
| 버블 | 데이터의 인접 요소끼리 비교하고, swap 연산을 수행하며 정렬하는 방식 |
| 선택 | 대상에서 가장 크거나 작은데이터를 찾아가 선택을 반복하면서 정렬하는 방식 |
| 삽입 | 대상을 선택해 정렬된 영역에서 선택 데이터의 적절한 위치를 찾아 삽입하면서 정렬하는 방식 |
| 퀵 | pivot 값을 선정해 해당 값을 기준으로 정렬하는 방식 |
| 병합 | 이미 정렬된 부분 집합들을 효율적으로 병합해 전체를 정렬하는 방식 |
| 기수 | 데이터의 자릿수를 바탕으로 비교해 데이터를 정렬하는 방식 |

### 버블 정렬

버블 정렬은 두 인접한 데이터의 크기를 비교해 정렬하는 방법이다. 간단하게 구현 가능하지만 구현할 순 있지만, 시간 복잡도는 O(n^2)으로 비효율적이다.

#### 수 정렬하기 1

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() 
{
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);

	int N;
	cin >> N;
	vector<int> arr(N);

	for (int i = 0; i < N; i++)
		cin >> arr[i];

	for (int i = 0; i < N; i++)
	{
		for (int j = 0; j < N - 1; j++)
		{
			if (arr[j] > arr[j + 1])
				swap(arr[j], arr[j + 1]);
		}
	}

	for (int i = 0; i < N; i++)
		cout << arr[i] << '\n';
}
```

### 선택 정렬

선택 정렬은 대상 데이터에서 최대나 최소 데이터를 나열된 순으로 찾아가며 선택하는 방법이다. 선택정렬은 구현 방법이 복잡하고 시간 복잡도도 O(n^2)으로 비효율적이라 코딩 테스트에서 많이 사용되지 않는다.

#### 내림차순으로 자릿수 정렬하기

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

int main()
{
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);

	string str;
	cin >> str;
	vector<int> A(str.size(), 0);

	for (int i = 0; i < str.size(); i++) {
		A[i] = stoi(str.substr(i, 1));
	}

	for (int i = 0; i < str.length(); i++) {
		int max = i;
		for (int j = i + 1; j < str.length(); j++) {
			if (A[max] < A[j])
				max = j;
		}
	}

	if (max != i)
		swap(A[max], A[i]);

	for (int i = 0; i < str.size(); i++)
		cout << A[i];
}
```

### 삽입 정렬

삽입 정렬은 이미 정렬된 데이터 범위에 정렬되지 않은 데이터를 적절한 위치에 삽입해 정렬하는 방식이다. 시간 복잡도는 O(n^2)이지만, 구현하기 쉽다.

### 퀵 정렬

퀵 정렬은 기준값을 선정해 해당 값보다 작은 데이터와 큰 데이터와 큰 데이터로 분류하는 것을 반복해 정렬하는 알고리즘이다. 기준값이 어떻게 선정되는지가 시간 복잡도에 많은 영향을 미치고 평균 시간 복잡도는 O(nlogn)이다. 최악의 경우 시간 복잡도는 O(n^2)이다.

#### K번째 수 구하기

```cpp
#include <iostream>
#include <vector>
using namespace std;

void quickSort(vector<int> &A, int S, int E, int K);
int partition(vector<int> &A, int S, int E);
void swap(vector<int> %A, int i, int j);

int main()
{
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);

	int N, K;
	cin >> N >> K;
	vector<int> A(N);

	for (int i = 0; i < N; i++)
		cin >> A[i];

	quickSort(A, 0, N - 1, K);
	cout << A[K - 1];
}

void quickSort(vector<int> &A, int S, int E, int K)
{
	int pivot = partition(A, S, E);
	if (pivot == k) {
		return;
	}
	else if (k < pivot) {
		quickSort(A, S, pivot - 1, K);
	}
	else {
		quickSort(A, pivot + 1, E, K);
	}
}

int partition(vector<int> &A, int S, int E)
{
	if (S + 1 == E) {
		if (A[S] > A[E])
			swap(A, S, E);
		return E;
	}

	int M = (S + E) / 2;
	swap(A, S, M);
	int pivot = A[S];
	int i = S + 1, j = E;

	while (i <= j) {
		while (j >= S + 1 && A[j] > pivot) { 
			j--;
		}
		while (i <= E && A[i] < pivot) {
			i++;
		}
		if (i < j) {
			swap(A, i, j);
		}
		else {
			break;
		}
	}

	A[S] = A[j];
	A[j] = pivot;
	return j;
}

void swap(vector<int> &A, int i, int j)
{
	int temp = A[i];
	A[i] = A[j];
	A[j] = temp;
}
```

### 병합 정렬

병합 정렬은 분할 정복 방식을 사용해 데이터를 분할하고 분할한 집합을 정렬하며 합치는 알고리즘이다. 시간 복잡도는 O(nlogn)이다.

2개의 그룹을 병합하는 과정은 투 포인터의 원리로 코딩테스트에서 두 배열을 한 배열로 합치는 문제에서도 사용된다.

#### 수 정렬하기 2

```cpp
#include <iostream>
#include <vector>
using namespace std;

void mergeSort(int S, int E);
static vector<int> A;
static vector<int> B;

int main()
{
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);

	int N;
	cin >> N;
	A.resize(N + 1);
	B.resize(N + 1);

	for (int i = 1; i <= N; i++)
		cin >> A[i];

	mergeSort(1, N);

	for (int i = 1; i <= N; i++)
		cout << A[i] << '\n';
}

void mergeSort(int S, int E)
{
	if (S == E)
		return;

	int M = (S + E) / 2;
	mergeSort(S, M);
	mergeSort(M + 1, E);

	int i = S, j = M + 1, k = S;
	while (i <= M && j <= E)
	{
		if (A[i] <= A[j])
			B[k++] = A[i++];
		else
			B[k++] = A[j++];
	}

	while (i <= M)
		B[k++] = A[i++];
	while (j <= E)
		B[k++] = A[j++];

	for (int i = S; i <= E; i++)
		A[i] = B[i];
}
```

### 기수 정렬

기수 정렬은 값을 비교하지 않는 특이한 정렬이다. 기수 정렬은 값을 놓고 비교할 자릿수를 정한 다음 해당 자릿수만 비교한다. 기수 정렬의 시간 복잡도는 O(kn)으로 k는 자릿수의 최대값이다.

#### 수 정렬하기 3

```cpp
#include <iostream>
using namespace std;

int main()
{
	ios::sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);

	int N;
	cin >> N;
	int A[10001] = { 0 };

	for (int i = 0; i < N; i++)
	{
		int num;
		cin >> num;
		A[num]++;
	}

	for (int i = 1; i <= 10000; i++)
	{
		for (int j = 0; j < A[i]; j++)
			cout << i << '\n';
	}
}
```
