## 7. 상속과 합성에 익숙해지기

상속과 합성은 객체지향 시스템설계에서 중요한 역할을 한다.

실제로도 어렵고 흥미로운 많은 부분 (논란도 많은)이 상속과 합성 사이에서 결정된다.

객체지향 설계 방식이 발전함에 따라 일부 개발자는 상속을 아예 배제하기도 한다.

행위를 직접 상속하기보다 인터페이스를 상속하는 편이 더 일반적이다.

데이터/모델을 주로 상속받고, 행위는 주로 구현하는 경향이 있다.

그럼에도 상속과 합성은 여전히 재사용 메커니즘이다.

상속이라는 이름에서 알 수 있듯이 실제 부모/자식 관계가 있는 다른 클래스의 특성과 행위를 상속한다.

합성은 객체들을 사용해 또 다른 객체를 작성하는 일까지 포함하는 개념이다.

### 7.1. 객체 재사용

상속과 합성이 존재하는 주된 이유는 객체를 재사용하기 위해서이다.

상속과 합성을 통해 기존 클래스를 활용해 새로운 클래스를 만들 수 있는데 이는 사실 미리 만들어 둔 클래스를 재사용하기 위한 유일한 방법이다.

- 상속: is-a의 관계로 개는 포유동물이다.
- 합성: has-a의 관계로 자동차에는 엔진이 있다.

합성은 다른 클래스를 사용해 더 복잡한 클래스를, 즉 일종의 어셈블리를 구축하는 작업이 포함된다.

이 경우 자식, 부모 관계가 없으며 기본적으로 복합 객체를 구성하는 방법이다.

처음 객체지향이 등장했을 때 상속이 가장 객체지향의 큰 장점으로 여겨졌다.

상속이 재사용의 본질이고, 상속은 재사용의 궁극적인 표현이였다.

하지만 시간이 지남에 따라 합성을 통해 설계해야 한다고 주장하는 사람들이 많아졌다.

그렇다고 상속을 배제하라는 것은 아니다.

상속은 효과적인 기술이며, 객체지향 설계에서 중요한 역할을 한다.

레거시 코드 유지보수나 개발자 툴킷에서 상속을 사용하는 것은 매우 효과적이다.

상속이 종종 오용되거나 남용되는 이유는 상속이 설계 전략으로 사용되는 근본적인 결함 때문이 아니고 **상속이 무엇인지 제대로 이해하지 못했기 때문이다.**

결론은 둘 다 잘 사용할 줄 알아야 한다는 점이고 객체지향 시스템 구축에 강력한 기술이라는 것이다.

따라서 우리는 두 가지 장단점을 모두 이해하고 적절한 맥락에서 이들을 사용하기 위해 시간을 들여야 한다.

### 7.2. 상속

> 상속을 사용하여 부모의 기능이 보장된다고 해서 테스트 코드를 작성하지 않아도 되는 것은 아니다.  

가장 많이 등장하는 문제점인 펭귄의 예로 설명한다.

행위를 지역적으로 오버라이딩할 수 있지만, 펭귄은 여전히 fly라는 메서드를 가지고 있을 것이다.

이런 의미없는 메서드를 사용하는 것은 SOLID원칙 중 LSP를 위반하는 것이다.

따라서 날지 않는 새와 같이 처음 계층 구조에서 한번 더 상속 구조로 내려가는 게 바람직할 수 있다.

하지만 실제로는 위와 같은 과정은 매우 복잡해진다..

#### 7.2.1. 일반화와 특수화

*generalization-specialization*

Dog라는 단일 클래스에서 다양한 개 품종을 고려하여 구현된다면 이는 일반화-특수화라고 불린다.

상속을 이용할 때 고려해야할 중요 사항이다.

이는 상속 트리를 따라 내려갈수록 사물들이 더 특수화된다는 생각이다.

트리의 맨 위가 가장 일반적인 개념이고, 아래로 내려갈수록 특수화된다.

상속이라는 개념은 상속이란 공통적인 성질을 배제해 나가면서 일반화에서 특수화로 나아가는 일이다.

#### 7.2.2. 설계 결정

이론적으로 보면 가능한 한 많은 공통성을 고려하는 편이 바람직하다.

그러나 모든 설계 문제와 마찬가지로 때로는 이게 지나치게 좋아서 오히려 부담이 될 수도 있다.

공통성을 최대한 고려하면 현실 세계에 최대한 가깝게 표현할 수 있지만, 모델에 대해서는 근접하게 표현하지 못할 수도 있다.

> **컴퓨터가 잘하지 못하는 것**  
> 분명히 컴퓨터 모델이 현실 세계의 상황을 근사할 수 는 있다.  
> 컴퓨터는 숫자 처리에 능숙하지만, 더 추상적인 작업에는 적합하지 않다.

현실은 불확실한 경우가 더 많음

더 큰 시스템에선 가능한 한 사물들을 단순하게 유지하는 것이 좋다.

덜 복잡하게 설계할지, 아니면 더 많은 기능을 갖추도록 설계할지는 일종의 균형잡기와 같다.

시스템이 너무 복잡해서 그 무게만으로 붕괴될 정도의 복잡성을 추가하지 않으면서 유연한 시스템을 구축하는 게 일차적인 목표이다.

설계 시에는 항상 상충 관계에 부딪히게 되는데 미래를 너무 염두하여 설계하거나 바로바로 현재에서 설계하는 둥의 균형잡기가 매우 어렵다..

#### 7.2.3. 합성

객체가 그 밖의 객체들을 포함할 수 있다는 생각은 자연스럽다.

컴퓨터의 그래픽카드, 하드디스크 등등의 구성 또는 합성은 쉽게 이해가 가능하다.

이는 해당 객체들이 각각 독립적으로 동작한다고 이해하기 때문이다.

특정 객체가 다른 객체로 합성되고 해당 객체가 객체 필드로 포함될 때마다 새 객체를 복합체나 응집체또는 컴포지션인 객체라고 한다.

#### 7.2.4. UML로 합성 표현하기

UML로 표현하는 방법을 알려준다.

표준이 아니고 최근에는 잘 사용하지 않기 때문에 따로 정리하지 않는다.

### 7.3. 캡슐화가 객체지향의 기본이 되는 이유

캡슐화는 객체지향의 기본 개념이다.

인터페이스/구현부라고 하는 패러다임을 다룰 때면 우리는 사실 캡슐화에 대해서 이야기 하는 것이다.

기본적인 질문은 클래스에서 무엇을 노출해야 하고 무엇을 노출해서는 안 되는가에 관한 것이다.

이 캡슐화는 데이터 및 행위와 동등한 관계에 놓여 있다.

어떤 클래스의 설계에 관한 결정을 논의할 때에는 데이터와 행위를 잘 작성된 클래스로 캡슐화하는 일에 초점을 맞춰야 한다.

캡슐화는 객체지향의 기본 요소이므로 객체지향 설계의 기본 규칙 중 하나인 것이다.

상속은 또한 세 가지 주요 객체지향 개념 중 하나로 간주된다.

그러나 어떤 면에서 보면 실제로는 상속이 캡슐화를 방해한다.

어떻게 이럴 수 있을까?

#### 7.3.1. 상속이 캡슐화를 약화시키는 방법

캡슐화는 공개 인터페이스 부분과 비공개 임플리멘테이션 부분으로 구분해서 따로따로 모아 두는 과정이다.

본질적으로 클래스는 자신이 아닌 그 밖의 클래스가 알 필요가 없는 모든 것을 숨긴다.

사례중 한 가지는 상속으로 인해 그 밖의 클래스에 대해서는 강력하게 캡슐화되는 꼴이 되지만, 정작 슈퍼클래스와 서브클래스 사이의 캡슐화는 약해지는 경우를 말한다.

문제는 슈퍼클래스에서 구현부를 상속한 후에 해당 구현부를 변경해 버리면 이러한 슈퍼클래스 내의 변경 내용이 클래스 위계구조를 통해 파급된다는 점이다.

이렇게 줄줄이 전파되는 효과는 잠재적으로 모든 서브클래스에 영향을 미친다.

이러한 파급효과는 예상치 못한 문제를 일으킬 수 있다.

즉, 파급 클래스에 변경된 내용이 투명하게 드러나지 않기 때문에 문제가 발생할 수 있다.

#### 7.3.2. 다형성의 자세한 예

많은 사람들이 다형성(Polymorphism)을 객체지향 설계의 초석이라고 생각한다.

완전히 독립적인 객체를 만들 목적으로 클래스를 설계하는 것이 객체지향의 핵심이다.

잘 설계된 시스템에서 객체는 그것에 관한 모든 중요한 질문에 답할 수 있어야 한다.

일반적으로 객체는 스스로 어떤 역할을 해야한다.

이 독립성은 코드 재사용의 기본 메커니즘 중 하나이다.

다형성은 많은 모양을 의미하고 메세지 객체로 전송될 때 객체는 해당 메세지에 응답하도록 정의된 메서드를 가져야 한다.

상속 위계구조에서 모든 자식 클래스는 해당 슈퍼클래스에서 인터페이스를 상속한다.

그러나 각 자식 클래스는 별도의 엔터티이므로 동일한 메세지의 대한 별도의 응답이 필요할 수 있다.

#### 7.3.3. 객체 책임성

추상클래스는 인스턴스화할 수 없지만, 파생클래스는 인스턴스화할 수 있다.

따라서 각 추상 클래스의 메서드를 각 파생클래스의 성격에 맞게 구현해야 한다.

그렇기에 다형성이 필요하고 전제는 다양한 객체에 메세지를 보낼 수 있다.

### 7.4. 결론

존경받는 객체지향 설계자들은 될 수 있으면 기본적으로 합성을 사용하면서 필요할 때만 상속을 사용해야 한다고 했다.

그러나 합성을 반드시 상속보다 많이 사용해야 하는 건 아니다.

뭐가 맞다 틀리다는 없으며, 상황에 맞춰 적절하게 섞어 쓰는 것이 올바르다.

### 느낀점

상속과 합성에 관한 내용으로 좀 더 깊게 생각하게 되는 챕터였다.

일반적으로 생각하는 상속에 문제점에 대해서도 조심스러운 입장과 왜 그런 문제점이 생기는 지 예제로 설명되어 있어서 도움이 되기도 했고 무작정 합성을 주로 사용하려고 하는데 그것도 문제가 될 수 있다는 것을 알게 되었다.