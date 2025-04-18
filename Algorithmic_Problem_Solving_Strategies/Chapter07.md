## 7장 분할 정복

분할 정복(Divide & Conquer)은 가장 유명한 알고리즘 디자인 패러다임으로, 각개 격파라는 말로 설명이 가능하다. 이 디자인 패러다임을 차용한 알고리즘은 **주어진 문제를 둘 이상의 부분 문제로 나눈 뒤 각 문제에 대한 답을 재귀 호출을 통해 계산하고, 각 부분 문제의 답으로부터 전체 문제의 답을 계산**해 낸다. 분할 정복이 일반적인 재귀호출가 다른 점은 문제를 한 조각과 나머지 전체로 나누는 대신 거의 같은 크기의 부분 문제로 나누는 것이다.

![image](https://github.com/fkdl0048/BookReview/assets/84510455/7ee6cc00-7cc4-4664-8133-1830d2973688)

*그림에서 알 수 있듯이 세로가 더 짧다.*

분할 정복을 사용하는 알고리즘들은 대개 세 가지의 구성 요소를 가지고 있다.

- 문제를 더 작은 문제로 분할하는 과정(divide)
- 각 문제에 대해 구한 답을 원래 문제에 대한 답으로 병합하는 과정 (merge)
- 더이상 답을 분할하지 않고 곧장 풀 수 있는 매우 작은 문제(base case)

*위 분할정복의 알고리즘은 마치 잘 구현된 코드와 같은 형태를 가져야 한다. (클래스의 세분화)*

위 특성에서 알 수 있듯이 분할 정복을 적용하기 위해선 문제에 몇 가지 특성이 성립해야 한다. 문제를 둘 이상의 부분 문제로 나누는 자연스러운 방법이 있어야 하며, 부분 문제의 답을 조합해 원래 문제의 답을 계산하는 효율적인 방법이 있어야 한다. 적용하기 어려워 보이는 조건을 가지지만 분할 정복은 같은 작업을 더 빠르게 처리할 수 있다는 장점이 있다.

### 예제: 수열의 빠른 합과 행렬의 빠른 제곱

앞서 재귀 호출에 대해서 다룰 때 `recursiveSum()`이라는 함수를 통해 `1 + 2 + ... + n`을 다뤘는데 이를 분할정복을 이용해 `fastSum()`함수를 만들어 보자. 1부터 n까지의 합을 n개의 조각으로 나눈 뒤, 이들을 반으로 뚝 잘라 n/2개의 조각들로 만들어진 부분 문제 두 개를 만든다. (편의상 n은 짝수로 가정)

$$ fastSum() = 1 + 2 + \cdots + n $$
$$ = (1 + 2 + \cdots + \frac{n}{2}) + ((\frac{n}{2} + 1) + \cdots + n) $$

첫 번째 부분 문제는 fastSum(n/2)로 나타낼 수 있지만, 두 번째 부분 문제는 그렇지 않다. 문제를 재귀적으로 풀기 위해서는 각 부분 문제를 '1부터 n까지의 합' 꼴로 표현할 수 있어야 하는데, 두 번째 조각은 'a부터 b까지의 합' 형태를 가지고 있기 때문이다. 따라서 다음과 같이 두 번째 부분 문제를 fastSum(x)를 포함하는 형태로 바꿔 써야 한다.

$$ (\frac{n}{2}+1)+\dotsb+n=(\frac{n}{2}+1)+(\frac{n}{2}+2)+\dotsb+(\frac{n}{2}+\frac{n}{2})$$

$$ =\frac{n}{2}\times\frac{n}{2}+(1+2+3+\dotsb+\frac{n}{2})$$
$$ =\frac{n}{2}\times\frac{n}{2}+ fastSum(\frac{n}{2})$$

따라서 다음과 같이 쓸 수 있다.

$$ fastSum(n) = 2 \times fastSum(\frac{n}{2}) + \frac{n^2}{4} $$

이를 코드로 구현하면 다음과 같다.

```cs
// 필수 조건: n은 자연수
// 1 + 2 + ... + n을 반환한다.
int fastSum(int n) {
    // 기저 사례(base case)
    if (n == 1) return 1;
    if (n % 2 == 1) return fastSum(n-1) + n;
    return 2*fastSum(n/2) + (n/2)*(n/2);
}
```

#### 시간복잡도 분석

fastSum()은 recursiveSum()에 비해서 훨씬 호출횟수가 적을 것이라 예상할 수 있는데, 이는 호출할 때마다 두 번에 한 번 꼴로 n이 절반으로 줄어들기 때문이다.

#### 행렬의 거듭제곱

n x n 크기의 행렬 A가 주어질 때, A의 거듭제곱(power)A^m는 A를 m번 곱한 것을 의미한다. 이것을 계산하는 것은 크게 어려울 것이 없지만, m이 매우 클 때 A^m을 계산하는 것은 어려울 수 있다.

*분할정복은 매우 빠른 속도로 구할 수 있다.*

위 아이디어를 사용하여 A^m을 구하는데 필요한 m개의 조각을 절반으로 나눠 보자.

$$ A^m = A^{m/2} \times A^{m/2} $$

이를 코드로 구현하면 다음과 같다.

```cs
// 정방 행렬을 표현하는 SquareMatrix 클래스가 있다고 가정한다.
// A^m을 반환한다.
SquareMatrix pow(const SquareMatrix& A, int m) {
    // 기저 사례: A^0 = I
    if (m == 0) return identity(A.size());
    if (m % 2 > 0) return pow(A, m-1) * A;
    SquareMatrix half = pow(A, m/2);
    return half * half;
}
```

#### 나누어 떨어지지 않을 때의 분할과 시간 복잡도

m이 홀수일 때, A^m=AxA^(m-1)로 나누지 않고, 좀더 절반에 가깝게 나누는 게 좋지 않을까라는 생각을 할 수도 있다. 예를 들어 7을 6,1이 아닌 3,4로 나누는 것이다. 실제로 문제의 크기가 절반에 가깝게 줄어들면 기저 사례에 도달하기까지 걸리는 분할의 횟수가 줄어들기 때문에 대부분의 분할 정복 알고리즘은 가능한 한 절반에 가깝게 문제를 나누고자 한다.

*실제로 퀵 정렬에서 좀더 좋은 분할을 찾기 위해 여러 노력을 하는 것과 같다.*

하지만 이 문제의 경우 분할은 오히려 알고리즘을 더 느리게 만든다. A^m을 찾기 위해 계산해야 할 부분 문제의 수가 늘어나기 때문이다.

![image](https://github.com/fkdl0048/BookReview/assets/84510455/47243978-8846-4134-9ecd-3a3bdbb402d1)

같은 문제라도 어떻게 분할하느냐에 따라 시간 복잡도 차이가 커진다는 것을 보여주는 좋은 예이다. **절반으로 나누는 알고리즘이 큰 효율 저하를 불러오는 이유는 바로 여러 번 중복되어 계산되면서 시간을 소모하는 부분 문제들이 있기 때문이다**. 이런 속성을 부분 문제가 중복된다라고 부른다.
(이후에 동적계획법이 고안된 계기가 된다.)

### 예제: 병합 정렬과 퀵 정렬

주어진 수열의 크기 순서대로 정렬하는 문제는 전산학에서 가장 유명한 문제로 정리된다. 그 중 가장 유명한 두 알고리즘은 병합 정렬과 퀵 정렬이다.

이 두 알고리즘 모두 분할 정복 패러다임을 기반으로 만들어진 것들이다.

*구현에 대한 자료는 인터넷에 많아서 생략*

- [정렬 관련 알고리즘 정리](https://github.com/fkdl0048/CodeReview/blob/main/Algorithm/Sort/README.md)

병합 정렬 알고리즘은 주어진 수열을 가운데에서 쪼개 비슷한 크기의 수열 두 개로 만든 뒤 이들을 재귀 호출을 이용해 각각 정렬한다. 그 후 정렬된 배열을 하나로 합침으로써 정렬된 전체 수열을 만들어 낸다.

```cs
private void MergeAndSort(int[] a, int p, int r)
{
    if (p < r)
    {
        int q = (p + r) / 2; // 절반으로 분활
        MergeAndSort(a, p, q); // 재귀 호출 결국 1개로 정렬된 리스트를 만듬
        MergeAndSort(a, q + 1, r);
        Merge(a, p, q, r);
    }
}
```

반대로 퀵 정렬 알고리즘은 배열을 단순하게 가운데에서 쪼개는 대신, 병합 과정이 필요 없도록 한쪽의 배열에 포함된 수가 다른 쪽 배열의 수보다 항상 작도록 배열을 분할합니다. 이를 위해 퀵 정렬은 파티션이라고 부르는 단계를 도입하는데, 이는 배열에 있는 수 중 임의의 '기준 수(pivot)'를 지정한 후 기준보다 작거나 같은 숫자를 왼쪽, 더 큰 숫자를 오른쪽으로 보내는 과정을 말한다.

```cs
private void QuickSortLogic(int[] a, int p, int r)
{
    if (p < r)
    {
        int q = Partition(a, p, r);
        QuickSortLogic(a, p, q - 1);
        QuickSortLogic(a, q + 1, r);
    }
}
```

![image](https://github.com/fkdl0048/ToDo/assets/84510455/0a496360-eee6-470c-bcb6-cb8614468aa3)

병합 정렬의 동작 과정은 각 수열의 크기가 1이 될 때까지 절반씩 쪼개 나간 뒤, 정렬된 부분 배열들을 합쳐 나가는 것을 볼 수 있다. 병합 정렬의 분할 방식은 아주 단순하고 효율적이다. 주어진 배열에서 가운데를 절반으로 나누는 것이다. 따라서 이 과정은 상수 시간인 O(1)이 걸린다. 그러나 각각 나눠서 정렬한 배열들을 하나의 배열로 합치기 위해 별도의 병합 과정을 실행해야 한다. 이 과정은 O(n)의 시간이 걸린다.

퀵 정렬은 각 부분 수열의 맨 처음에 있는 수를 기준으로 삼고, 이들보다 작은 수를 왼쪽으로, 큰 것을 오른쪽으로 가게끔 문제를 분해한다. 이 분할은 O(n)의 시간이 걸리는 복잡한 작업인데다, 어떤 기준을 선택하느냐에 따라 분할의 결과가 매우 달라질 수 있다.

이 두 알고리즘은 같은 아이디어로 정렬을 수행하지만 시간이 많이 걸리는 작업을 분할을 분할 단계에서 하느냐, 병합 단계에서 하느냐가 다르다. 이렇게 같은 문제를 해결하는 알고리즘이라도 어떤 식으로 분할하느냐에 따라 다른 알고리즘이 될 수 있으며, 이들은 모두 분할 정복 패러다임을 사용한 알고리즘이다.

#### 시간 복잡도 분석

병합 정렬과 퀵 정렬의 시간 복잡도는 비슷한 방법으로 볼 수 있다. O(n)의 시간이 걸리는 과정을 재귀 호출 전에 하느냐, 후에 하느냐가 다를 뿐 본질적으로 비슷한 형태의 알고리즘이기 때문이다.

병합 정렬은 각 단계마다 반으로 나눈 부분 문제를 재귀 호출을 이용해 해결한 뒤, 이들의 결과 수열을 합쳐 전체 문제의 답을 계산한다. 정렬된 두 부분 수열을 합쳐 전체 문제의 답을 계산한다. 정렬된 두 부분 수열을 합치는 데는 두 수열의 길이 합만큼 반복문을 수행해야 하기 때문에, 병합 정렬의 수행 시간은 이 병합 과정에 의해 지배된다.

그런데 아래 단계로 내려갈수록 부분 문제의 수는 두 배로 늘고 각 부분 문제의 크기는 반으로 줄어들기 때문에, 한 단계 내에서 모든 병합에 필요한 총 시간은 O(n)으로 항상 일정하다. 필요한 단계의 수는 O(logn)이 되며, 따라서 병합 정렬의 시간 복잡도가 O(nlogn)이 된다.

퀵 정렬의 경우 대부분의 시간을 차지하는 것은 주어진 문제를 두 개의 부분 문제로 나누는 파티션 과정이다. 파티션에는 주어진 수열의 길이에 비례하는 시간이 걸리므로, 사실 병합 정렬에서의 병합 과정과 다를 것이 없다. 병합 정렬과 달리 퀵 정렬의 시간 복잡도를 분석하기 까다로운 것은 결과적으로 분할된 두 부분 문제가 비슷한 크기로 나눠진다는 보장을 할 수 없기 때문이다.

최악의 경우엔 O(n^2)이 걸릴 수 있다. 다행이 평균적으로 부분 문제가 절반에 가깝게 나눠질 때 퀵정렬의 시간 복잡도는 병합 정렬과 같은 O(nlogn)이 된다. 그래서 대부분의 퀵 정렬 구현은 가능한 한 절반에 가까운 분할을 얻기 위해 좋은 기준을 뽑는 다양한 방법들을 제공한다.

### 예제: 카라츠바의 빠른 곱셈 알고리즘

분할 정복 알고리즘의 한 예가 카라츠바의 빠른 곱셈 알고리즘으로, 두 개의 정수를 곱하는 알고리즘이다. 물론 32비트 정수 둘을 곱할 때 쓰는 것은 아니고, 수백자리, 나아가 수만 자리는 되는 큰 숫자들을 다룰 때 주로 사용한다.

이렇게 큰 숫자들은 배열을 이용해 저장해야 한다. 두 자연수의 십진수 표기가 배열에 주어진다고 할 때, 이 둘을 곱한 결과를 계산하는 가장 기본적인 방법은 초등학교 때 배운 방법과 같다.

두 정수 1234와 5678을 곱하는 과정을 예로 본다.

```cs
int[] multiply(int[] a, int[] b) {
    int[] c = new int[a.length + b.length + 1];
    for (int i = 0; i < a.length; i++)
        for (int j = 0; j < b.length; j++)
            c[i+j] += a[i] * b[j];
    normalize(c);
    return c;
}

void normalize(int[] num) {
    num[num.length-1] = 0;
    for (int i = 0; i < num.length-1; i++) {
        if (num[i] < 0) {
            int borrow = (Math.abs(num[i]) + 9) / 10;
            num[i+1] -= borrow;
            num[i] += borrow * 10;
        } else {
            num[i+1] += num[i] / 10;
            num[i] %= 10;
        }
    }
}
```

`multiply()`가 일반적인 정수형 변수가 아닌 정수형 배열을 입력받는 점을 눈여겨봐야 한다. 이 배열들은 곱할 수의 각 자릿수를 맨 아래 자리부터 저장하고 있다. 이렇게 순서를 뒤집으면 입출력은 불편하지만 주어진 자릿수의 크기를 10^i로 쉽게 구할 수 있다는 장점이 있다.

결국 O(n^2)의 시간이 걸리는데, 이를 O(n^1.585)로 줄일 수 있는 카라츠바의 빠른 곱셈 알고리즘이 있다. (1960년에 등장)

카라츠바의 빠른 곱셈 알고리즘은 두 수를 각각 절반으로 쪼갠뒤, a와 b가 각각 256자리 수라면 a_1과 b_1은 첫 128자리, a_0과 b_0은 나머지 128자리가 된다.

$$ a = a_1 \times 10^{128} + a_0 $$
$$ b = b_1 \times 10^{128} + b_0 $$

카라츠바는 이때 a x b를 네 개의 조각을 이용해 표현하는 방법을 살펴보았다. 예를 들어 다음과 같이 표현할 수 있다.

$$ a \times b = (a_1 \times 10^{128} + a_0) \times (b_1 \times 10^{128} + b_0) $$
$$ = a_1 \times b_1 \times 10^{256} + (a_1 \times b_0 + a_0 \times b_1) \times 10^{128} + a_0 \times b_0 $$

이 방법에서는 우리는 큰 정수 두개를 한 번 곱하는 대신, 절반 크기로 나눈 작은 조각을 네 번 곱한다.

*10의 거듭제곱을 곱하는 것은 그냥 뒤에 0을 붙이는 시프트 연산으로 구현하면 되니 곱셈으로 치지 않는다.*

이대로도 각각을 재귀 호출해서 해결하면 분할 정복 알고리즘이라고 할 수 있다. 하지만 이 경우 실제 전체 수행 시간이 O(n^2)이 되어버린다. 이를 해결하기 위해 카라츠바는 다음과 같은 방법을 사용했다.

a x b를 표현했을 때 네 번 대신 세 번의 곱셈으로만 이 값을 계산할 수 있다는 것이다.

$$ a \times b = a_1 \times b_1 \times 10^{256} + (a_1 \times b_0 + a_0 \times b_1) \times 10^{128} + a_0 \times b_0 $$

조각들의 곱을 각각 3개로 나눠서 계산하면 다음과 같이 표현할 수 있다.

$$ a \times b = z_2 \times 10^{512} + (z_1 - z_2 - z_0) \times 10^{256} + z_0 $$

이를 코드로 구현하면 다음과 같다.

```cs
z2 = a1 * b1;
z0 = a0 * b0;
z1 = (a1 + a0) * (b1 + b0) - z0 - z2;
```

이 과정에서 곱셈을 세 번밖에 쓰지 않기에 다시 적절하게 조합하여 답을 구할 수 있다.

```cs
int[] karatsuba(int[] a, int[] b) {
    int an = a.length, bn = b.length;
    if (an < bn) return karatsuba(b, a);
    if (an == 0 || bn == 0) return new int[0]; // 기저 사례: 어느 한쪽이 0일 경우
    if (an <= 50) return multiply(a, b); // 기저 사례: a와 b가 모두 50자리 이하일 경우 O(n^2)의 시간복잡도를 가지는 곱셈 알고리즘을 사용한다.
    int half = an / 2;
    int[] a0 = Arrays.copyOfRange(a, 0, half);
    int[] a1 = Arrays.copyOfRange(a, half, an);
    int[] b0 = Arrays.copyOfRange(b, 0, Math.min(b.length, half));
    int[] b1 = Arrays.copyOfRange(b, Math.min(b.length, half), b.length);
    int[] z2 = karatsuba(a1, b1);
    int[] z0 = karatsuba(a0, b0);
    addTo(a0, a1, 0);
    addTo(b0, b1, 0);
    int[] z1 = karatsuba(a0, b0);
    subFrom(z1, z0);
    subFrom(z1, z2);
    int[] ret = new int[an + bn - 1];
    addTo(ret, z0, 0);
    addTo(ret, z1, half);
    addTo(ret, z2, half + half);
    return ret;
}
```

카라츠바 알고리즘은 분할한 부분 문제의 답에서 원래 문제의 답을 병행해는 부분을 개선함으로써 알고리즘 성능을 향상시킨 좋은 예이다.

#### 시간 복잡도 분석

카라츠바 알고리즘은 두 개의 입력을 절반씩으로 쪼갠 뒤, 세 번 재귀 호출을 하기 때문에 재귀 호출은 한 번이나 두번 만 하던 지금과는 다르게 분석해야 한다.

우선 카라츠바 알고리즘의 수행 시간을 병합 단계와 기저 사례의 두 부분으로 나눈다. 병합 단계의 수행 시간은 addTo()와 subFrom()의 수행 시간에 지배되고, 기저 사례의 처리 시간은 multiply()의 수행 시간에 지배된다.

먼저 기저 사례를 처리하는 데 드는 총 시간을 알아본다. 다 긴 숫자의 길이가 50자리보다 짧아지면 multiply()를 이용해 답을 계산하기로 했지만, 여기서는 편의를 위해 한 자리 숫자에 도달해야만 multiply()를 사용한다고 가정한다.

### [예제: 쿼드 트리 뒤집기](https://algospot.com/judge/problem/read/QUADTREE#)

대량의 좌표 데이터를 메모리 안에 압축해 저장하기 위해 사용하는 여러 기법 중 쿼트 트리(쿼드 트리)라는 것이 있다. *옥 트리도 있으며 쿼드 트리는 2D 게임에서 옥트리는 3D 게임에서 쓰인다.*

가장 많이 쓰이는 예로는 흑백 이미지 압축에 많이 사용한다고 한다. 이미지를 4등분하여 각각의 부분이 모두 흑색이면 하나의 흑색으로 압축하고, 하나라도 흰색이면 하나의 흰색으로 압축한다. 이 과정을 분할 정복을 이용하면 된다.

- 그림의 모든 픽셀이 검은 색이라면 크기에 관게없이 b로 압축
- 그림의 모든 픽셀이 흰 색이라면 크기에 관계없이 w로 압축
- 그렇지 않다면, 크기를 4등분하여 각각의 부분을 쿼드 트리 압축

가장 쉽게 생각할 수 있는 접근법은 뒤집기 위해서 압축을 풀어서 실제 이미지를 얻고, 상하 반전을 한 뒤 다시 쿼드 트리를 압축하는 방법이 있다.

#### 압축 풀고 분할하기

```cpp
char decompressed[MAX_SIZE][MAX_SIZE];
void decompress(string::iterator& it, int y, int x, int size) {
    char head = *(it++);
    if (head == 'b' || head == 'w') {
        for (int dy = 0; dy < size; ++dy)
            for (int dx = 0; dx < size; ++dx)
                decompressed[y+dy][x+dx] = head;
    } else {
        int half = size / 2;
        decompress(it, y, x, half);
        decompress(it, y, x+half, half);
        decompress(it, y+half, x, half);
        decompress(it, y+half, x+half, half);
    }
}
```

이 경우에는 2^20 * 2^20 크기의 이미지를 다루는 것이므로 거의 1테라바이트에 달하는 크기가 된다. 분할정복을 사용했지만 좀 더 효율적인 방법이 필요하다.

#### 압축 다 풀지 않고 뒤집기

이 경우에는 이미지를 뒤집은 결과를 곧장 생성하는 코드가 필요하다. 즉, 압축해제 과정 없이 이미지를 뒤집은 결과를 바로 출력할 수 있어야 한다.

문제의 특징을 조금 살펴보면 사실 흰색이나 검은색으로 가득 찬, w나 b는 뒤집을 필요가 없다. 전체가 한 가지 색이 아닌 경우에만 이들을 병합하여 답을 얻어야 한다. 그림의 분할되기 전 즉 압축 상태에서 각각 상하로 뒤집은 결과를 얻고 이를 다시 합치면 된다.

```cpp
string reverse(string::iterator& it) {
    char head = *it;
    ++it;
    if (head == 'b' || head == 'w')
        return string(1, head);
    string upperLeft = reverse(it);
    string upperRight = reverse(it);
    string lowerLeft = reverse(it);
    string lowerRight = reverse(it);
    return string("x") + lowerLeft + lowerRight + upperLeft + upperRight;
}
```

### [예제: 울타리 잘라내기](https://algospot.com/judge/problem/read/FENCE#)

이 문제는 N개의 나무 판자를 붙여 세운 울타리가 있다. 울타리를 재구성하기 위해 가장 큰 정사각형을 만드는 문제이다.

최대값을 구하는 문제이기 때문에 무식하게 풀이는 가능하지만 성능에 제한이 있기 때문에 분할 정복을 사용해야 한다.

#### 무식하게 풀기

```cpp
int bruteForce(const vector<int>& h) {
    int ret = 0;
    int N = h.size();
    for (int left = 0; left < N; ++left) {
        int minHeight = h[left];
        for (int right = left; right < N; ++right) {
            minHeight = min(minHeight, h[right]);
            ret = max(ret, (right - left + 1) * minHeight);
        }
    }
    return ret;
}
```

#### 분할 정복을 이용한 풀이

분할 정복 알고리즘에 맞게 설계하기 위해선 n개의 판자를 절반으로 나눠 두 개의 부분 문제를 만든다. 그렇다면 찾는 최대 직사각형은 세 가지 중 하나에 속한다.

- 가장 큰 직사각형을 왼쪽 부분 문제에서만 잘라낼 수 있다.
- 가장 큰 직사각형은 오른쪽 부분 문제에서만 잘라낼 수 있다.
- 가장 큰 직사각형은 왼쪽 문제와 오른쪽 부분 문제에 걸쳐 있다.

이렇게 3번 째의 경우만 답을 구할 수 있으면 분할정복의 기본 아이디어를 적용할 수 있다.

```cpp
int solve(const vector<int>& h, int left, int right) {
    if (left == right) return h[left];
    int mid = (left + right) / 2;
    int ret = max(solve(h, left, mid), solve(h, mid+1, right));
    int lo = mid, hi = mid + 1;
    int height = min(h[lo], h[hi]);
    ret = max(ret, height * 2);
    while (left < lo || hi < right) {
        if (hi < right && (lo == left || h[lo-1] < h[hi+1])) {
            ++hi;
            height = min(height, h[hi]);
        } else {
            --lo;
            height = min(height, h[lo]);
        }
        ret = max(ret, height * (hi - lo + 1));
    }
    return ret;
}
```

### [예제: 팬미팅](https://algospot.com/judge/problem/read/FANMEETING#)

팬미팅에서 멤버들과 팬들이 만나서 포옹을 하는데, 멤버들은 팬들보다 훨씬 크다. 멤버들과 팬들이 포옹을 하는 순간이 몇 번인지 구하는 문제이다.

#### 곱셈으로 변형

이 문제를 푸는 방법은 앞서 적용한 카라츠바의 곱셍을 이용하면 매우 쉽게 풀린다.

```cpp
int hugs(const string& members, const string& fans) {
    int N = members.size(), M = fans.size();
    vector<int> A(N), B(M);
    for (int i = 0; i < N; ++i) A[i] = (members[i] == 'M');
    for (int i = 0; i < M; ++i) B[M-i-1] = (fans[i] == 'M');
    vector<int> C = karatsuba(A, B);
    int allHugs = 0;
    for (int i = N-1; i < M; ++i)
        if (C[i] == 0)
            ++allHugs;
    return allHugs;
}
```
