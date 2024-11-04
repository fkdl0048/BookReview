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

#### ATM 인출 시간 계산하기

