## Item 32: public 상속 모형은 반드시 "is-a(...는 ...의 일종이다)"를 따르도록 만들자

다른 OOP언어와 다르게 C++에서 OOP는 좀 더 생각해야 할 부분이 많다. 상속의 경우에는 단일 상속과 다중 상속이 가능하고, 상속 관계가 하나하나가 public, private, protected의 성질을 가질 수 있다. 추가로 가상 상속과 비가상 상속도 얹힐 수 있다.

이러한 관계들에서 대해서 C++에서는 명확하게 규칙을 알고 있어야 한다.

- 가상 함수 상속 -> 인터페이스만 상속
- 비가상 함수 상속 -> 인터페이스와 구현을 상속

클래스 `D("Derived")`를 클래스 `B("Base")`로부터 public 상속을 통해 파생시켰다면, C++ 컴파일러는 다음과 같이 이해한다. **D 타입으로 만들어진 모든 객체는 B 타입의 객체이지만, 그 반대는 되지 않는다. B는 D보다 일반적인 개념을 나타내며, D는 B보다 특수한 개념을 나타낸다.**

그래서 B 타입의 객체가 쓰일 수 있는 모든곳에는 D 타입의 객체도 쓰일 수 있다고 단정하는 것이다. D 타입의 모든 객체는 B 타입의 객체도 되기 때문이다. 하지만 이 반대는 불가능하다. D는 B의 일종이지만, B는 D의 일종이 아니다.

실제로 상속이란 개념에 맞게, `is-a`를 만족하는 것이 중요하다는 것인데, 실제 코드에서 상속의 세계는 매우 엄격하다. 예로 펭귄이나 죽음의 다이아몬드, 정사각형의 문제 등이 상속의 문제점을 잘 보여준다.

*따라서 인터페이스를 통한 has-a관계를 통한 객체 합성을 사용하는 것이 더 좋다.*

상속관계는 문법적하자보다 상속의 개념이 들어가야 하는 설계의 영역이기에 더욱 명심하는 것이 좋다.

클래스들 사이에 맺을 수 있는 관계로 is-a만 있는 것은 아니다. has-a와 is-implemeted-in-terms-of도 있다. 이 두가지는 Item38 및 Item39에서 다룬다.

### 정리

- public 상속의 의미는 "is-a" 관계를 나타낸다. 기본 클래스에 적용되는 모든 것들이 파생 클래스에 그대로 적용되어야 한다. 왜냐하면 모든 파생 클래스 객체는 기본 클래스 객체의 일종이기 때문이다.