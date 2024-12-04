## Item 24: 타입 변환이 모든 매개변수에 대해 적용되어야 한다면 비멤버 함수를 선언하자

> 클래스에서 암시적 타입 변환을 지원하는 것은 일반적으로 못된 생각이다.

앞서 머리말에서 나온 이야기이다. 다만 숫자타입에서는 예외가 존재한다. 이때 사용자가 정의한 클래스의 매개 변수에 대한 타입 변환을 자연스럽게 지원하겠다고 생각할 수 있다. (숫자 타입이라면 int -> double의 흐름은 정상적이다.)

앞서 Item23의 내용처럼 비멤버 함수로 해당 타입변환 연산을 수행하라는 것이 이번 아이템의 주제이다.

우선 기본적으로 사용자 정의 매개변수끼리의 연산(함수)는 쉽게 동작한다. 다만 혼합형에 대해서 동작하고자 할 때 암시적 형변환에 의해서 문제가 발생한다. 주어진 기본 숫자형이 만약 인자로 주어진다면 함수 쪽에선 사용자 정의 타입을 요구한다는 사실을 알고 있다.

```cpp
result = oneHalf * 2;
// 아래와 같음 (Rational은 사용자 정의 클래스)
// const Rational temp(2);
// result = oneHalf * temp;
```

이렇게 컴파일러가 동작한 이유는 명시호출로 선언되지 않은 생성자가 있기 때문이다. 만약 명시 호출 생성자가 있었다면 제대로 컴파일되지 않았을 것이다.

**여기서 핵심은 암시적 타입 변환에 대해 매개변수가 먹혀들려면 매개변수 리스트에 들어 있어야만 한다는 것이다.** 따라서 혼합형 수치 연산(타입 변환)을 수행하고 싶다면 비멤버 함수로 만들어서 컴파일러 쪽에서 모든 인자에 대해서 암시적 타입 변환을 수행하도록 내버려 두는 것이다.

이 함수도 Item23번과 마찬가지로 public인터페이스만을 써서 구현할 수 있으므로 프렌드로 둘 필요가 없다.

> 멤버 함수의 반대는 프렌드 함수가 아니라 비멤버 함수이다.

### 정리

- 어떤 함수에 들어가는 모든 매개변수에 대해 타입 변환을 해 줄 필요가 있다면, 그 함수는 비멤버이어야 한다.

템플릿(제네릭)을 사용하면 되지 않을까? 했는데, Item46에서 다룬다고 한다.