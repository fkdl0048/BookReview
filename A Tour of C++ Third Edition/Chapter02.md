## 2장: 사용자 정의 타입

> 당황하지 말자!  
> 더글라스 아담스

기본 타입과 const한정자, 선언자 연산자로 만들 수 있는 타입을 내장 타입이라 부른다. C++는 풍부한 내장 타입과 연산 집합을 제공하지만 **일부러 저수준에서 제공한다.** 따라서 일반적인 컴퓨터 하드웨어의 능력을 직접적으로 그리고 효율적으로 가져다 쓴다. 따라서 다른 언어나 고수준 애플리케이션을 작성하는 기능들은 제공하지 않는다. 따라서 내장 타입과 연산에 추상 메커니즘을 추가하여 프로그래머가 이러한 고수준 기능을 만들 수 있도록 했다.

이름과 참조를 통해 struct 멤버에 접근할 때는 `.`을 포인터를 통해 접근할 때는 `->`를 사용한다.

표현과 연산을 밀접하게 관련시켜 용법을 단일화하고 데이터의 일관성을 보장하고, 향후 표현을 개선할 수 있도록 사용자가 표현에 접근하지 못하게 하고 싶을 때가 많다._이를 위해 클래스가 등장했다. 클래스는 멤버들의 집합이며, 멤버는 데이터나 함수, 타입 멤버일 수 있다.

