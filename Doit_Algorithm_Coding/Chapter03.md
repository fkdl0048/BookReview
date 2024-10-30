## 자료구조

### 배열과 리스트 그리고 벡터

기본 자료구조인 배열과 리스트는 비슷한 점도 많지만 다른 점도 많다. 두 자료구조의 특징을 정확하게 이해하고 문제가 요구하는 조건에 따라 적절하게 선택해 사용하는 것이 중요하다.

#### 배열과 리스트의 핵심 이론

##### 배열

배열은 메모리의 연속 공간에 값이 채워져 있는 형태의 자료구조로 배열의 값은 인덱스를 통해 참조할 수 있다. 선언한 자료형의 값만 저장할 수 있으며 크기가 정해져 있어 크기를 초과하는 값을 저장할 수 없다.

- 특징
  - 인덱스를 사용하여 값에 바로 접근할 수 있다.
  - 새로운 값을 삽입하거나 특정 인덱스에 있는 값을 삭제하기 어렵다. 값을 삽입하거나 삭제하려면 해당 인덱스 주변에 있는 값을 이동시키는 과정이 필요하다.
  - 배열의 크기는 선언할 때 지정할 수 있으며, 한 번 선언하면 크기를 늘리거나 줄일 수 없다.
  - 구조가 간단하므로 코딩 테스트에서 자주 사용된다.

##### 리스트

리스트는 값과 포인터를 묶은 노드라는 것을 포인터로 연결한 자료구조다.

- 특징
  - 인덱스가 없으므로 값에 접근하려면 Head 포인터부터 순서대로 접근해야 한다. 즉, 값에 접근하는 속도가 느리다.
  - 포인터로 연결되어 있으므로 데이터를 삽입하거나 삭제하는 연산 속도가 빠르다.
  - 선언할 때 크기를 별도로 지정하지 않아도 된다. 다시 말해 리스트의 크기는 정해져 있지 않으며, 크기가 변하기 쉬운 데이터를 다룰 때 적절하다.
  - 포인터를 저장할 공간이 필요하므로 배열보다 구조가 복잡하다.

##### 벡터

벡터는 C++ 표준 라이브러리에 있는 자료구조 컨테이너 중 하나로, 사용자가 손 쉽게 사용하기 위해 정의된 클래스다. 기존의 배열과 같은 특징을 가지면서 배열의 단점을 보완한 동적 배열의 형태라고 생각하면 된다.

- 특징
  - 동적으로 원소를 추가할 수 있다. 즉, 크기가 자동으로 늘어난다.
  - 맨 마지막 위치에 데이터를 삽입하거나 삭제할 때는 문제가 없지만 중간 데이터의 삽입 삭제는 배열과 같은 메커니즘으로 동작한다.
  - 배열과 마찬가지로 인덱스를 이용하여 각 데이터에 직접 접근할 수 있다.

```cpp
vector<int> A;  // int형 벡터 A 선언

A.push_back(1); // A의 맨 뒤에 1 추가
A.insert(A.begin(), 7); // A의 맨 앞에 7 추가
A.insert(A.begin() + 2, 10); // A의 2번째 위치에 10 추가

A[4] = 3; // A의 4번째 값을 3으로 변경

A.pop_back(); // A의 맨 뒤 값을 삭제
A.erase(A.begin() + 2); // A의 2번째 값을 삭제
A.clear(); // A의 모든 값을 삭제

A.size(); // A의 크기 반환
A.front(); // A의 맨 앞 값 반환
A.back(); // A의 맨 뒤 값 반환
A[3]; // A의 3번째 값 반환
A.at(3); // A의 3번째 값 반환
A.begin(); // A의 시작 위치 반환
A.end(); // A의 끝 위치 반환
```

#### 숫자의 합 구하기

```cpp
#include <iostream>
using namespace std;

int main() 
{
	int N = 0;
	string numbers;
	
	cin >> N;
	cin >> numbers;

	int sum = 0;
	for (int i = 0; i < numbers.length(); i++) {
		sum += numbers[i] - '0';
	}

	cout << sum << "\n"
}
```

##### C++에서의 형 변환

> string형에서 숫자형으로

```cpp
#include <string>

string sNum = "123";
string sNum_d = "123.45";

int iNum = stoi(sNum); // 123
long lNum = stol(sNum); // 123
double dNum = stod(sNum_d); // 123.45
float fNum = stof(sNum_d); // 123.45
```

> 숫자형에서 string형으로

```cpp
#include <string>
int iNum = 123;
long lNum = 123;
double dNum = 123.45;
float fNum = 123.45;

string sNum = to_string(iNum); // "123"
string sNum = to_string(lNum); // "123"
string sNum = to_string(dNum); // "123.45"
string sNum = to_string(fNum); // "123.45"
```

#### 평균 구하기

```cpp
#include <iostream>
using namespace std;

int main()
{
	int N = 0;
	int A[1000];
	cin >> N;

	for (int i = 0; i < N; i++) {
		cin >> A[i];
	}

	long sum = 0;
	long max = 0;

	for (int i = 0; i < N; i++) {
		if (max < A[i]) {
			max = A[i];
		}
		sum += A[i];
	}

	double result = sum * 100.0 / max / N;
	cout << result << "\n";
}
```

### 구간 합

구간 합은 합 배열을 이용하여 시간 복잡도를 더 줄이기 위해 사용하는 특수한 목적의 알고리즘이다.

#### 구간 합의 핵심 이론

구간 합 알고리즘을 활용하려면 먼저 합 배열을 구해야 한다. 배열 A가 있을 때 합 배열 S는 다음과 같이 정의된다.

`S[i] = A[0] + A[1] + ... + A[i]`

합 배열은 기존의 배열을 전처리한 배열이라고 생각하면 되고, 합 배열을 미리 구해 놓으면 기존 배열의 일정 범위의 합을 구하는 시간 복잡도가 O(N)에서 O(1)로 감소한다.

*DP의 기본 원칙같은 느낌*

합 배열을 만드는 공식

`S[i] = S[i - 1] + A[i]`

합 배열을 이용한 구간 합을 구하는 공식

`S[j] - S[i - 1]`

#### 구간 합 구하기 1

```cpp
#include <iostream>
using namespace std;

int main() 
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int suNo, quizNo;
	cin >> suNo >> quizNo;
	int S[100001] = {};

	for (int i = 1; i <= suNo; i++) {
		int num;
		cin >> num;
		S[i] = S[i - 1] + num;
	}

	for (int i = 0; i < quizNo; i++) {
		int start, end;
		cin >> start >> end;
		cout << S[end] - S[start - 1] << "\n";
	}
}
```

#### 구간 합 구하기 2

- 2차원 구간 합 배열 `D[X][Y]` 정의
  - `D[X][Y] = 원본 매열의 (0, 0)부터 (X, Y)까지의 합`

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int N, M;
	cin >> N >> M;

	vector<vector<int>> D(N + 1, vector<int>(N + 1, 0));
	vector<vector<int>> S(N + 1, vector<int>(N + 1, 0));

	for (int i = 1; i <= N; i++) {
		for (int j = 1; j <= N; j++) {
			cin >> D[i][j];
			S[i][j] = S[i - 1][j] + S[i][j - 1] - S[i - 1][j - 1] + D[i][j];
		}
	}

	for (int i = 0; i < M; i++) {
		int x1, y1, x2, y2;
		cin >> x1 >> y1 >> x2 >> y2;
		cout << S[x2][y2] - S[x1 - 1][y2] - S[x2][y1 - 1] + S[x1 - 1][y1 - 1] << "\n";
	}
}
```

#### 나머지 합 구하기

- 나머지 합 문제 풀이의 핵심
  - (A + B) % M = (A % M + B % M) % M 와 같다는 것 즉, 특정 구간 수들의 나머지 연산을 더해 나머지 여난을 한 값과 이 구간 합의 나머지 연산을 한 값은 동일하다.
  - 구간 합 배열을 이용한 식 S[j] - S[i]는 원본 배열의 i + 1부터 j까지의 구간 합이다.

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int N, M;
	cin >> N >> M;

	vector<long> S(N, 0);
	vector<long> C(M, 0);
	long answer = 0;
	cin >> S[0];

	for (int i = 1; i < N; i++) {
		int temp;
		cin >> temp;
		S[i] = S[i - 1] + temp;
	}

	for (int i = 0; i < N; i++) {
		int remainder = S[i] % M;
		
		if (remainder == 0) {
			answer++;
		}

		C[remainder]++;
	}

	for (int i = 0; i < M; i++) {
		answer += C[i] * (C[i] - 1) / 2;
	}

	cout << answer << "\n";
}
```

### 투 포인터

투 포인터는 2개의 포인터로 알고리즘의 시간 복잡도를 최적화한다.

#### 연속된 자연수의 합 구하기

```cpp
#include <iostream>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int N;
	cin >> N;
	int count = 1;
	int start_index = 1;
	int end_index = 1;
	int sum = 1;

	while (start_index <= N) {
		if (sum < N) {
			end_index++;
			sum += end_index;
		}
		else if (sum > N) {
			sum -= start_index;
			start_index++;
		}
		else {
			count++;
			end_index++;
			sum += end_index;
		}
	}

	cout << count << "\n";
}
```

#### 주몽의 명령

*시간복잡도를 보고 정렬이 사용가능하다면 사용하여 접근*

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int N, M;
	cin >> N >> M;
	vector<int> A(N, 0);

	for (int i = 0; i < N; i++) {
		cin >> A[i];
	}
	sort(A.begin(), A.end());

	int count = 0;
	int left = 0;
	int right = N - 1;

	while (left < right) {
		if (A[left] + A[right] == M) {
			count++;
			left++;
			right--;
		}
		else if (A[left] + A[right] < M) {
			left++;
		}
		else {
			right--;
		}
	}
}
```

#### '좋은 수' 구하기

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int N;
	cin >> N;
	vector<int> A(N, 0);

	for (int i = 0; i < N; i++) {
		cin >> A[i];
	}

	sort(A.begin(), A.end());

	int result = 0;
	for (int k = 0; k < N; k++) {
		long find = A[k];
		int i = 0;
		int j = N - 1;

		while (i < j) {
			if (i == k) {
				i++;
				continue;
			}
			if (j == k) {
				j--;
				continue;
			}

			if (A[i] + A[j] == find) {
				result++;
				break;
			}
			else if (A[i] + A[j] < find) {
				i++;
			}
			else {
				j--;
			}
		}
	}
}
```

### 슬라이딩 윈도우

슬라이딩 윈도우 알고리즘은 2개의 포인터로 범위를 지정한 다음, 범위를 유지한 채로 이동하며 문제를 해결한다. 투포인터와 유사하지만 슬라이딩 윈도우는 범위를 유지하며 이동하는 것이 특징이다.

#### DNA 비밀번호

```cpp
#include <iostream>
using namespace std;

int checkArr[4];
int myArr[4];
int checkSecret = 0;
void Add(char c);
void Remove(char c);

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int S, P;
	cin >> S >> P;
	int Result = 0;
	string A;
	cin >> A;

	for (int i = 0; i < 4; i++) {
		cin >> checkArr[i];
		if (checkArr[i] == 0) {
			checkSecret++;
		}
	}

	for (int i = 0; i < P; i++) {
		Add(A[i]);
	}
	if (checkSecret == 0) {
		Result++;
	}

	for (int i = P; i < S; i++) {
		int j = i - P;
		Add(A[i]);
		Remove(A[j]);

		if (checkSecret == 4) {
			Result++;
		}
	}

	cout << Result << "\n";
}

void Add(char c)
{
	if (c == 'A') {
		myArr[0]++;
		if (myArr[0] == checkArr[0]) {
			checkSecret++;
		}
	}
	else if (c == 'C') {
		myArr[1]++;
		if (myArr[1] == checkArr[1]) {
			checkSecret++;
		}
	}
	else if (c == 'G') {
		myArr[2]++;
		if (myArr[2] == checkArr[2]) {
			checkSecret++;
		}
	}
	else if (c == 'T') {
		myArr[3]++;
		if (myArr[3] == checkArr[3]) {
			checkSecret++;
		}
	}
}

void Remove(char c)
{
	if (c == 'A') {
		if (myArr[0] == checkArr[0]) {
			checkSecret--;
		}
		myArr[0]--;
	}
	else if (c == 'C') {
		if (myArr[1] == checkArr[1]) {
			checkSecret--;
		}
		myArr[1]--;
	}
	else if (c == 'G') {
		if (myArr[2] == checkArr[2]) {
			checkSecret--;
		}
		myArr[2]--;
	}
	else if (c == 'T') {
		if (myArr[3] == checkArr[3]) {
			checkSecret--;
		}
		myArr[3]--;
	}
}
```

### 스택과 큐

스택과 큐는 배열에서 조금 더 발전한 형태의 자료구조이다.

#### 스택과 큐의 핵심 이론

스택은 삽입과 삭제 연산이 후입선출(LIFO)로 이뤄지는 자료구조이다.

큐는 삽입과 삭제 연산이 선입선출(FIFO)로 이뤄지는 자료구조이다. *BFS에서 사용*

> 우선순위 큐라는 개념도 있지만, 이는 들어간 순서에 상관없이 우선순위가 높은 데이터가 먼저 나오는 자료구조이다. 큐 설정에 따라 front에 항상 최대값 또는 최솟값이 위치한다. 우선순위 큐는 일반적으로 힙을 이용해 구현하는데, 힙은 트리 종류 중 하나이므로 개념정도만 이해하자.

#### 스택으로 수열 만들기

```cpp
#include <iostream>
#include <stack>
#include <vector>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int N;
	cin >> N;
	vector<int> A(N, 0);
	vector<char> result;

	for (int i = 0; i < N; i++) {
		cin >> A[i];
	}

	stack<int> s;
	int num - 1;
	bool result = true;

	for (int i = 0; i < A.size(); i++) {
		int su = A[i];

		if (su >= num) {
			while (su >= num) {
				s.push(num++);
				result.push_back('+');
			}
			s.pop();
			result.push_back('-');
		} else {
			int n = s.top();
			s.pop();
			if (n > su) {
				cout << "NO\n";
				result = false;
				break;
			} else {
				result.push_back('-');
			}
		}
	}

	if (result) {
		for (int i = 0; i < result.size(); i++) {
			cout << result[i] << "\n";
		}
	}
}
```

#### 오큰수 구하기

```cpp
#include <iostream>
#include <stack>
#include <vector>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	int N;
	cin >> N;
	vector<int> A(N, 0);
	vector<int> result(N, -1);

	for (int i = 0; i < N; i++) {
		cin >> A[i];
	}

	stack<int> s;
	s.push(0);

	for (int i = 1; i < N; i++) {
		while (!s.empty() && A[s.top()] < A[i]) {
			result[s.top()] = A[i];
			s.pop();
		}
		s.push(i);
	}

	while (!s.empty()) {
		result[s.top()] = -1;
		s.pop();
	}

	for (int i = 0; i < N; i++) {
		cout << result[i] << " ";
	}
	cout << "\n";
}
```

#### 카드 게임

```cpp
#include <iostream>
#include <queue>
using namespace std;

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	queue<int> q;
	int N;
	cin >> N;

	for (int i = 1; i <= N; i++) {
		q.push(i);
	}

	while (q.size() > 1) {
		q.pop();
		q.push(q.front());
		q.pop();
	}

	cout << q.front() << "\n";
}
```

#### 절대값 힙 구현하기

```cpp
#include <iostream>
#include <queue>
using namespace std;

struct compare {
	bool operator()(int o1, int o2) {
		int first_abs = abs(o1);
		int second_abs = abs(o2);
		if (first_abs == second_abs) {
			return o1 > o2;
		} else {
			return first_abs > second_abs;
		}
	}
};

int main()
{
	ios_base::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);

	priority_queue<int, vector<int>, compare> pq;
	int N;
	cin >> N;

	for (int i = 0; i < N; i++) {
		int num;
		cin >> num;
		if (num == 0) {
			if (pq.empty()) {
				cout << "0\n";
			} else {
				cout << pq.top() << "\n";
				pq.pop();
			}
		} else {
			pq.push(num);
		}
	}
}
```