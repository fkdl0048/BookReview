## 4장. 알고리즘의 시간 복잡도 분석

> 어떤 작업이 주어졌을 때 컴퓨터가 이 작업을 해결하는 방법을 가리켜 알고리즘이라고 한다.  

- 지하철 2호선을 타고 시청역으로 간다.
- 지하철 1호선으로 갈아타고 청량리역으로 간다.
- 경춘선을 타고, 춘천역에서 내린다.

- 강동구 쪽으로 가는 버스를 탄다.
- 동서울 버스 터미널 근처에 온 것 같으면 내린다.
- 춘천 쪽으로 가는 버스를 타고, 한참 가다 내린다.

여기서 전자는 알고리즘이고, 후자는 알고리즘이 아니다.

객관적으로 전자에 비해 후자는 주관적이며, 본질적으로 모호하기 때문이다.

한 문제를 해결하는 데 여러 개의 알고리즘이 있을 수 있다면 그중 어떤 알고리즘을 만드는 법을 배워야 할까?

알고리즘을 평가하는 두 가지의 큰 기준은 알고리즘을 사용하는 시간과 공간이다.

- 시간: 알고리즘이 적은 시간을 사용한다는 것은 더 빠르게 동작한다는 이야기이다.
- 공간: 알고리즘이 적은 공간을 사용한다는 것은 더 적은 용량의 메모리를 사용한다는 이야기다.

이 두 기준은 상충하는 경우가 많다.

물론 더 빠르면서 메모리도 더 적게 사용하는 알고리즘이 있을 수 있지만, 대개는 시간과 공간 중 하나를 희생해야 한다.

대개 시간의 중요성을 더 많이 본다.

*트레이드 오프는 항상 발생한다.*

### 도입

좀더 빠른 알고리즘을 만들기 위해 가장 먼저 해야 할 일은 바로 알고리즘의 속도를 어떻게 측정할지를 정하는 것이다.

알고리즘의 속도를 측정할 수 없다는 것은 바꿨을 때 이것이 더 빨라졌는지 너 느려졌는지 알 수 없기 때문이다.

#### 반복문이 지배한다

대부분 알고리즘에서 시간을 지배하는 것은 반복문이다.

물론 입력에 상관없이 항상 같은 수행시간을 같는 알고리즘도 있지만, 대개는 입력의 크기에 따라 수행 횟수가 정해지는 반복문이 있기 마련이다.

수행 시간에 관한 이야기로 N과 N^2수행 시간 비교 코드가 있다.

### 선형 시간 알고리즘

#### 다이어트 현황 파악: 이동 편균 계산하기

책 예제 참고

수행시간이 N인 알고리즘을 선형 시간 알고리즘이라고 한다.

### 선형 이하 시간 알고리즘

#### 성현 전 사진 찾기

어떤 문제건 입력된 자료를 모두 한 번 훑어보는 데에는 입력의 크기에 비례하는 시간, 즉 선형 시간이 걸린다.

그럼 선형 시간보다 빠르게 동작하는 알고리즘은 자료를 다 보지도 않았는데 어떻게 가능할까?

이는 입력된 자료에 대해 이미 알고있는 지식을 활용한다면 가능하다.

이진 탐색의 경우가 이에 해당한다.

N장의 사진에서 성형 기준에 해당하는 사진을 찾는 문제를 생각하면 N시간이 아닌 logN시간이 걸린다.

#### 그래도 선형 시간 아닌가요?

앞 선 설명에 의해서 결국 사람이 모든 사진을 보면서 성형 이전인지 여부를 판단해야 한다.

게다가 그 사진들을 전부 오름차순으로 정렬해야 한다.

이는 이진 탑색 자체의 알고리즘에서 고려할 필요가 없는 부분이다.

입력으로 주어지는 성형 판단과 정렬되어서 들어오는 사진들은 알고리즘의 일부가 아니다.

#### 구현

간단한 아이디어와 달리 아진 탐색을 정확하게 구현하기는 매우 까다롭다.

### 지수 시간 알고리즘

#### 다항 시간 알고리즘

변수 N과 N^2, 그외 N의 거듭제곱의 선형 결합으로 이루어진 식들을 다항식이라고 부른다.

반복문의 수행 횟수를 입력 크기의 다항식으로 표햔할 수 있는 알고리즘들을 다항 시간 알고리즘이라고 부른다.

#### 알러지가 심한 친구들

여러 개의 답이 있는 문제가 있을 수 있다.

이 때 모든 경우의 수를 찾아서 답을 할 수 있지만, 더 나은 답이며 구체적으로는 한 종류의 답만 알아내고 싶다면 가장 간단한 방법은 모든 답을 일일이 고려해보는 것이다.

마치 모든 경우를 트리형태로 나열한 뒤, 뻗어나가게 되는 것인데 이를 가장 쉽게 구현하는 방법은 재귀 호출을 이용하는 것이다.

#### 지수 시간 알고리즘

지수 형태와 같은 알고리즘은 전체 수행 시간에 엄청난 영향을 미친다.

물론 개수가 적어서 티가 나지 않을 수 있지만, 개수가 늘어날 수록 기하급수적으로 늘어나기 때문이다.

이런 알고리즘을 지수 시간 알고리즘이라고 부른다.

#### 소인수 분해의 수행 시간

입력으로 주어지는 숫자의 개수가 아니라 그 크기에 따라 수행 시간이 달라지는 알고리즘들 또한 지수 수행 시간을 가질 수 있다.

소인수 분해의 경우가 그렇다.

### 시간 복잡도

이 내용은 알고리즘을 짜는데는 문제가 되지 않지만 개념 자체는 모든 곳에 사용하기 때문에 알아두는 것이 좋다.

시간 복잡도란 가장 널리 사용되는 알고리즘의 수행 시간 기준으로, 알고리즘이 실행되는 동안 수행하는 기본적인 연산의 수를 입력의 크기에 대한 함수로 표현한 것이다.

기본적인 연산이란 더 작게 쪼갤 수 없는 최소 크기의 연산이라고 생각하면 된다.

- 두 부호 있는 32비트 정수의 사칙연산
- 두 실수형 변수의 대소 비교
- 한 변수에 다른 변수 대입하기

반면 다음과 같은 연산들은 반복문을 포함하기 때문에 기본적인 연산이 아니다.

- 정수 배열 정렬하기
- 두 문자열이 서로 같은지 확인하기
- 입력된 수 소인수 분해하기

앞 절에선 가장 깊이 중첩된 반복문의 수행 횟수를 계산했다.

가장 깊이 중첩된 반복문의 내부에 있는 기본적인 연산들은 더 쪼갤 수 없기 때문에, 이것이 시간 복잡도의 대략적인 기준이 된다.

시간 복잡도가 높다는 말은 입력의 크기가 증가할 때 알고리즘의 수행 시간이 더 빠르게 증가한다는 의미

*시간 복잡도가 낮다고 해서 언제나 더 빠르게 동작하는 것은 아니다.*

입력의 크기가 충분히 작을 때는 시간 복잡도가 높은 알고리즘이 더 빠르게 동작할 수도 있다.

반대로 시간 복잡도가 낮은 알고리즘은 입력이 커지면 커질수록 더 효육적이 된다.

#### 입력의 종류에 따른 수행 시간의 변화

입력의 크기가 수행 시간을 결정하는 유일한 척도는 아니다.

입력이 어떤 형태로 구성되어 있는지도 수행 시간에 영향을 미친다.

*배열의 경우 반복문이 실행되는 횟수는 찾는 원소의 위치에 따라 달라진다.*

#### 접근적 시간 표기: O 표기

시간 복잡도는 알고리즘의 수행 시간을 표기하는 방법이지만 복잡하다는 문제가 있다.

이를 간단하게 대문자 O 표기법을 사용하여 알고리즘의 수행 시간을 표기한다.

간단하게 주어진 함수에서 가장 빨리 증가하는 항만 남기고 다 버리는 표기법이다.

#### O 표기법의 의미

O 표기법은 계산하기 간편하고, 알아보기 쉽다.

단순하게 시간의 오래, 적당히, 금방을 나타내기 보다 대략적인 함수의 상한을 나타낸다는 의미가 있다.

이를 통해 가끔 알고리즘의 최악의 수행 시간을 알아냈다고 착각하는 일이 흔히 있다.

하지만 O표기법은 각 경우의 수행 시간을 간단히 나타내는 표기법일 뿐, 특별히 최악의 수행시간과 관련이 있는 것은 아니다.

### 수행 시간 어림짐작하기

#### 주먹구구 법칙

프로그래밍 대회의 시간 제한은 알고리즘의 시간 복잡도가 아니라 프로그램의 수행 시간을 기준으로 한다.

따라서 어떤 알고리즘이 이 문제를 해결할 수 있을지 알기 위해서는 프로그램을 작성하기 전에 입력의 최대 크기와 알고리즘의 시간 복잡도를 보고 수행 시간을 어림짐작할 수 있어야 한다.

*매우 어려운..*

그러나 많은 경우는 시간 복잡도와 입력 크기만 알고 있더라도 알고리즘이 시간 안에 동작할지 대략적으로 짐작하는 것이 가능하다.

> 입력의 크기를 시간 복잡도에 대입해서 얻은 반복문 수행 횟수에 대해, 1초당 반복문 수행 횟수가 1억(10^8)을 넘어가면 시간을 초과할 가능성이 높다.

#### 주먹구구 법칙은 주먹구구일 뿐이다

이 주먹구구 법칙은 수많은 가정 위에 지어진 사상누각이기 때문에, 절대로 맹신해서는 안된다.

### 계산 복잡도 클래스

시간 복잡도는 알고리즘의 특성이지 문제의 특성이 아니다.

한 문제를 푸는 두 가지 이상의 알고리즘이 있을 수 있고, 이들의 시간 복잡도는 각각 다를 수 있기 때문이다.

이때 각 문제에 대해 이를 해결하는 얼마나 빠른 알고리즘이 존재하는지를 기준으로 문제들을 분류하고 각 분류의 특성을 연구하는 학문이 있다.

이를 계산 복잡도 이론이라고 한다.

#### 문제의 특성 공부하기

계산 복잡도 이론은 각 문제의 특성을 공부하는 학문이다.

- 정렬 문제: 주어진 N개의 정수를 정렬한 결과는 무엇인가?
- 부분 집합 합 문제: N개의 수가 있을 때 이 중 몇 개를 골라내서 그들의 합이 S가 되도록 할 수 있을까?

이 두 문제 중 어느 것이 더 쉽거나 어려운지 분간할 수 있을까?

단 이 때 정의하는 문제의 '어려움'은 문제를 풀 때의 난이도가 아니다.

계산 복잡도 이론에서 문제의 난이도는 해당 문제를 해결하는 빠른 알고리즘이 있느냐를 나타낸다.

빠른 계산 알고리즘이 있는 문제는 계산적으로 쉽고, 없는 문제는 계산적으로 어렵다고 한다.

계산 복잡도 이론에서 이렇게 다항 시간 알고리즘이 존재하는 문제들의 집합을 P 문제라고 한다.

정렬문제는 P 문제로 P문제처럼 같은 성질을 갖는 문제들을 모아놓은 집합을 계산 복잡도 클래스라고 부른다.

#### 난이도의 함정

P가 다항 시간에 풀 수 있는 문제면, 그 짝인 NP는 당연히 풀 수 없는 문제여야 하지만 그렇지 않다.

계산 복잡도에서 흔히 말하는 어려운 문제들은 다음과 같이 정의된다.

- 정말 어려운 문제를 잘 골라서 이것을 어려운 문제의 기준으로 삼는다.
- 그리고 앞으로는 기준 문제만큼 어렵거나 그보다 어려운 문제들, 다시 말해 '기준 이상으로 어려운 문제들'만을 어렵다고 부른다.

*하지만 이런 기준은 매우 주관적이며 역설적으로 보일 수 있다.*

#### NP 문제, NP 난해 문제

어려운 문제의 기준을 SAT문제라고 한다.

SAT 문제란 N개의 불린 값 변수로 구성된 논리식을 참으로 만드는 변수 값들의 조합을 찾는 문제다.

SAT 문제를 왜 기준으로 삼았을까? (모든 NP문제 이상으로 어렵기 때문)

NP문제란, 답이 주어졌을 때 이것이 정답인지를 다항 시간 내에 확인할 수 있는 문제를 말한다.

NP 난해 문제란, NP 문제 이상으로 어려운 문제를 말한다.

#### P = NP?

밀레니엄 7대 수학 난제로 유명해진 P = NP 문제는 P와 NP가 같은 집합인지를 묻는 문제다.

NP-난해 문제 중 하나를 다항 시간에 풀 수 있다면, 이 알고리즘을 이용해 NP에 속한 모든 문제를 다항 시간에 해결할 수 있으므로 P = NP임을 알 수 있다.

그 반대로 NP문제 중 하나를 골라 P에 포함되어 있지 않음을, 다시 말해 다항 시간에 푸는 방법이 없음을 증명하면 P != NP임을 알 수 있다.

아직까지 미해결이라는 말은 둘 중 하나에 성공한 사람이 아직 아무도 없다는 뜻이다.

#### 주의사항

3장의 실수와 마찬가지로 더 방대한 내용을 이해하기 쉽게 압축한 내용이라 더 자세한 내용은 직접 알아보자!

### 느낀점

마찬가지로 시간복잡도에 개념에 대해서 궁금할 때 다시 보면 될 내용이다.

마치 초반 1~4장까지는 카탈로그를 보는 것 같다.