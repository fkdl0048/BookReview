## 12. 창발성

> 창발또는 떠오름 현상은 하위 계층에는 없는 특성이나 행동이 상위 계층에서 자발적으로 돌연히 출현하는 현상이다.

착실하게 따르기만 하면 우수한 설계가 나오는 간단한 규칙이 있다..?!

1. 모든 테스트를 실행한다.
2. 중복을 없앤다.
3. 프로그래머 의도를 표현한다.
4. 클래스와 메서드 수를 최소로 줄인다.

*중요도 순*

### 단순한 설계 규칙 1: 모든 테스트를 실행하라

설계는 의도한 대로 돌아가는 시스템을 내놓아야 한다.

문서로는 시스템을 완벽하게 설계했지만, 시스템이 의도한 대로 돌아가는지 검증할 간단한 방법이 없다면, 문서 작성을 위해 투자한 노력에 대한 가치는 인정받기 어렵다.

여기서 말하는 시스템은 테스트를 철저히 거쳐 모든 테스트 케이스를 항상 통과하는 시스템이다.

이어 말해서 테스트가 불가능한 시스템은 검증 또한 불가능하다.

검증을 하지 못한다면 해당 시스템은 출시가 불가능한 제품이다.

따라서 개발자는 테스트가 가능한 시스템을 만드려고 하는데 이때 설계 품질또한 같이 올라간다.

*단위 테스트에서 나온 관심사 분리*

크기가 작고 목적 하나만 수행하는 클래스가 나온다.

SRP를 준수하는 클래스는 테스트가 휠씬 더 쉽다.

테스트 케이스가 많을수록 개발자는 테스트가 쉽게 코드를 작성한다.

따라서 철저한 테스트가 가능한 시스템을 만들면 더 나은 설계가 얻어진다.

`테스트 케이스를 만들고 계속 돌려라`라는 간단한 규칙은 시스템 자체의 낮은 결합도와 높은 응집력이라는 객체지향 방법론이 지향하는 목표를 달성한다.

즉, **테스트 케이스를 작성하면 설계 품질이 높아진다**.

### 단순한 설계 규칙 2~4: 리팩터링

테스트 케이스를 모두 작성했다면, 이제 코드와 클래스를 정리할 차례이다.

구체적으로는 코드를 점진적으로 리팩터링 해나간다.

이때는 새로 추가하거나 변경하는 코드에 대해 설계에 대한 영향을 생각하며 가독성을 높이는 작업을 한다.

코드를 정리해가며 시스템이 깨질까 걱정할 필요가 전혀 없다. 테스트 케이스가 있으니까..!

응집도를 높이고, 결합도를 낮추고, 관심사를 분리하고, 시스템 관심사를 모듈로 나누고, 함수와 클래스 크기를 줄이고, 더 나은 이름을 선택하는 등 다양한 기법을 동원한다.

### 중복을 없애라

우수한 설계에서 중복은 커다란 적이다.

중복은 추가 작업, 추가 위험, 불필요한 복잡도를 뜻한다.

똑같은 코드는 물론이거니와 비슷한 코드도 중복이다.

중복을 없애는 과정에서도 마찬가지로 함수로 작성하게 되면서 하나의 목적 그리고 더 작게 만들게 된다.

그렇게 만든 클래스는 다시 보니 SRP를 위반하게 되고 더 작게 클래스로 분활되게 된다.

### 표현하라

자신이 이해하는 코드는 짜기 쉽다.

코드를 짜는 동안 문제에 몰입하여 코드를 작성하니..

하지만 나중에 코드를 유지보수할 사람이 코드를 짜는 사람만큼 문제를 이해할 가능성은 희박하다.

*여기서 말하는 다른 사람은 자신도 포함*

그러므로 코드는 개발자의 의도를 분명히 표현해야 한다.

개발자가 코드를 명백하게 짤수록 다른 사람이 그 코드를 이해하기 쉬워진다.

- 좋은 이름을 선택하라
- 함수와 클래스 크기를 가능한 줄인다.
- 표준 명칭을 사용한다.
- 단위 테스트 케이스를 꼼꼼히 작성한다.

프로그래머에게 있어서 코드를 몇 만줄짠건 사실 자랑이 아니다.

좋은 구조를 한번이라도 제대로 만들어본 경험이 더 비싸고 값진 것이다.

### 클래스와 메서드 수를 최소로 줄여라

중복을 제거하고, 의도를 표현하고, SRP를 준수한다는 기본적인 개념도 극단적으로 치달으면 득보다 실이 많아진다.

클래스와 메서드 크기를 줄이자고 조그만 클래스와 메서드를 수없이 만드는 경우도 많다.

그래서 이 규칙은 함수와 클래스 수를 가능한 줄이라고 말한다.

### 결론

경험을 대신할 단순한 개발 기법이 있을까?? 읍다..!

마찬가지로 책을 읽고 지식을 쌓는 것도 중요하지만 실제로 적용해보고 몸으로 느끼는 것이 제일 중요하다.

### 느낀점

앞서 나온 내용을 잘 정리해준 챕터인 것 같다.

클린코드에 대한 감이 슬슬 잡히는 것 같다.

#### 논의사항

테스트 코드를 알고 있었지만 왜 짜지 않을까..? 이렇게 유용하고 중요한데..

라는 의문에서 생각을 해봤는데 저는 사실 그냥 돌아가는 프로그램만 생각하고 넘겼던 것 같습니다.(귀찮다고 생각한게 1순위)

여러분들은 왜 테스트코드를 짜지 않는지.. 아직 시도해보지 않았다면 왜 짜지 않는지 생각해보시면 좋을 것 같습니다.