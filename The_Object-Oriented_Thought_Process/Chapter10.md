## 10. 디자인 패턴

업무용 소프트웨어 시스템을 작성하려면 개발자는 사업방식을 완전히 이해해야 한다.

결과적으로 개발자는 종종 회사의 업무 과정에 대해 가장 친밀한 지식을 갖게 된다.

우리가 포유류 클래스를 만들 때, 모든 포유류가 특정 행위와 특성을 공유하기 때문에 이 포유류 클래스를 사용하면 개나 고양이 등의 클래스들을 수없이 만들 수 있다.

이렇게 하다 보면 개, 고양이, 다람쥐 및 기타 포유류를 연구할 때 효과적인데, 이는 **패턴**을 발견할 수 있기 때문이다.

이런 패턴을 기반으로 특정 동물을 살펴보고 그게 포유류인지 아니면 행위와 특성의 패턴이 포유류와 다른 파충류인지 판단할 수 있다.

역사적으로 우리는 이러한 패턴 사용해왔다.  

이러한 패턴은 소프트웨어 분야의 중요한 부분으로 소프트웨어 재사용형태와 밀접한 관련이 있다.(디자인 패턴)

패턴은 재사용 가능한 소프트웨어 개발 개념에 아주 적합하다.

객체지향 개발은 모두 재사용에 관한 것이므로 패턴과 객체지향 개발은 함께 진보한다.

디자인 패턴의 기본 개념은 모범 사례의 원칙과 관련이 있다.

모범 사례에 따르면 우수하고 효율적인 솔루션이 만들어질 때, 이러한 솔루션은 다른 사람들이 실패로부터 배웠던 방식과 이전의 성공에 따른 이익을 얻을 수 있었던 방식을 문서화한다는 것을 의미한다.

### 10.1. 디자인 패턴이 필요한 이유

디자인 패턴의 개념이 반드시 재사용 가능한 소프트웨어의 필요성에서 시작된 것은 아니다.

디자인 패턴에 대한 중요한 업적은 건물과 도시 건설에 관련되어 있다.

패턴은 우리 환경에서 반복적으로 발생하는 문제들을 기술하며, 그 문제에 대한 핵심 해법도 기술하는데, 이런 방식 덕분에 우리는 그러한 해법을 백만 번 거듭해서 사용할 수 있으며, 그럼에도 같은 방식을 두 번 다시 따르지 않아도 된다..

#### 10.1.1. 패턴의 네 가지 요소

*GoF는 패턴을 다음 네 가지 필수 요소로 설명한다.*

- 패턴 이름
  - 패턴 이름은 디자인 문제, 솔루션 및 결과를 한두 단어로 설명하는 데 사용할 수 있는 조종간 같은 것
  - 패턴에 이름을 붙여두면 어휘가 즉시 늘어나며 너 높은 수준으로 설계를 추상화할 수 있다.
  - 패턴에 대한 어휘를 갖추면 동료와 소통할 수 있고 문서로 교신할 수 있으며 자신과도 이야기 할 수 있다.
  - 이해하기 쉬워지면 절충산 설계를 다른 사람들에게 쉽게 전달할 수 있다.
- 문제
  - 문제는 패턴을 적용할 시점을 설명한다.
  - 이것으로 문제 자체와 내용을 설명할 수 있다.
  - 알고리즘을 객체로 표현하는 방법과 같은 특정 설계 문제들을 설명할 수 있다.
  - 융통성이 없는 설계의 증상인 클래스 구조나 객체 구조를 설명할 수 있다.
- 해법
  - 해법은 설계, 관계, 책임 및 협업을 이루는 요소를 설명한다.
  - 패턴은 여러 상황에 적용할 수 있는 템플릿과 같기 때문에, 해법은 구체적인 특정 설계나 구현을 설명하지 않는다.
  - 그 대신 패턴으로는 설계 문제를 추상적으로 설명하며, 요소를 일반적으로 배치해 설계 문제를 어떻게 해결하는지 보여준다.
- 귀결
  - 귀결이란 패턴을 적용한 결과와 절충점을 말한다.
  - 귀결이 종종 무시되기도 하지만, 우리가 설게 결정 내용을 설명할 때는 귀결이 설계 대안을 평가하고 적용 패턴의 비용과 이점을 이해하는데 중요하다.
  - 소프트웨어 귀결은 시공간상의 절충과 관련이 있다.
  - 귀결로 언어 문제 및 구현 문제도 해결할 수 있다.

### 10.2. 스몰토크의 모델/뷰/컨트롤러

MVC는 종종 디자인 패턴의 기원을 설명하는 데 사용된다.

모델/뷰/컨트롤러(MVC)라는 패러다임은 스몰토크에서 사용자 인터페이스를 생성하는데 사용했다.

> 스몰토크  
> 스몰토크는 그 당시 가장 인기있는 객체지향 언어였다..

Design Patterns에서는 다음과 같은 방식으로 MVC 컴포넌트들을 정의한다.

모델(model)은 애플리케이션 객체이고 뷰(view)는 화면 표현이며, 컨트롤러(controller)는 사용자 인터페이스가 사용자 입력에 반응하는 방식을 정의한다.

이전 패러다임에서는 모델, 뷰, 컨트롤러가 단일 엔터티 안에 모두 모여 있는 문제가 있었다.

예를 들어, 단일 객체에 이 세 가지 컴포넌트가 모두 들어 있는 식이다.

MVC패러다임에서는 이 세 가지 컴포넌트에 개별적이며 구별되는 인터페이스들이 있다.

따라서 우리는 어떤 애플리케이션의 사용자 인터페이스를 변경하려고 한다면 뷰만 변경하면 된다.

이는 객체지향 개발의`인터페이스 대 구현부`와 관련이 있다.

가능한 인터페이스와 구현부를 분리하려고 한다.

또한 인터페이스들끼리도 서로 최대한 분리하려고 한다.

MVC는 아주 일반적이면서도 기본적인 프로그래밍 문제와 관련된 특정 컴포넌트 간의 인터페이스를 명시적으로 정의한다.

MVC 개념을 따르고 사용자 인터페이스, 비즈니스 로직 및 데이터를 분리하면서 시스템이 훨씬 유연하고 강력해진다.

예를 들어, 사용자 인터페이스가 클라이언트 시스템에 있고, 비즈니스 로직이 애플리케이션 서버에 있고, 데이터가 데이터 서버에 있다고 가정한다면 개발 도중에 비즈니스 로직에 영향 없이 GUI의 형태를 변경할 수 있다.

> **MVC의 단점**  
> MVC는 훌룡한 디자인이지만, 선행 디자인에 많은 주의를 기울여야 한다는 점에서 복잡할 수 있다.  
> 이는 일반적인 객체지향 디자인의 문제이다.  
> 좋은 디자인과 성가신 디자인 사이에는 미세한 경계선이 있다.  
> 그렇다면 과연 완전한 디자인과 관련해 시스템이 얼마나 복잡해야 하는가?  
> 정닶은 없고, 항상 트레이드 오프를 생각하자.

### 10.3. 디자인 패턴의 종류

Design Patterns에는 세 가지 범주로 분류된 총 23개의 패턴이 있다.

- 생성 패턴(creational patterns)
  - 객체를 직접 인스턴스화하지 않은 채로 객체를 만든다.  
  - 이를 통해 특정 사례에 대해 어떤 객체를 생성해야 할지 결정할 때 프로그램의 유연성이 향상된다.  
- 구조 패턴(structural patterns)
  - 복잡한 사용자 인터페이스나 계정 데이터와 같은 객체 그룹을 더 큰 구조로 합성할 수 있다.
- 행위 패턴(behavioral patterns)
  - 시스템의 객체 간 통신을 정의하고 복잡한 프로그램에서 흐름을 제어하는 방법을 정의한다.

#### 10.3.1. 생성 패턴

생성 패턴은 다음 범주별로 나눠볼 수 있다.

- 추상 팩토리(Abstract Factory)
- 빌더(Builder)
- 팩토리 메서드(Factory Method)
- 프로토타입(Prototype)
- 싱글톤(Singleton)

이 책에선 각 패턴을 자세하게 다루지 않고 디자인 패턴이 뭔지 설명하는 내용이 주를 이룬다.  

```
생각

확실히 멘토님께서 디자인 패턴먼저 공부하지 말라고 하신 이유를 다시한번 느낀다.  
게임 프로그래밍에 적합한 디자인 패턴도 물론 있지만 해당 디자인 패턴을 공부한다고 억지로 적용하면 오히려 독이다.

현재 구조에서 필요한 범주 생성인지, 행위인지, 구조인지 필요성을 느낄 때 해당 범주내에서 적합한 디자인 패턴을 공부하고 적용하는 것이 더 효울적이고 기억에 남는다는 것.

공부의 순서는 없지만 적합한 방법은 있는 것 같다.
```

##### 팩토리 메서드 디자인 패턴

객체 생성, 즉 인스턴스화는 객체지향 프로그래밍에서 가장 기본적인 개념 중에 하나일 수 있다.

해당 객체가 존재하지 않으면 객체를 사용할 수 없다는 것은 말할 필요도 없다.

코드를 작성할 때 객체를 인스턴스화하는 가장 확실한 방법은 new키워드를 사용하는 것이다.

```java
abstract class Shape{

}

class Circle extends Shape{

}

Circle circle = new Circle();
```

이 코드는 문제없이 동작하지만, 코드에서 Circle이나 해당 문제의 다른 도형들을 인스턴스화 해야 할 곳이 많을 것이다.

대부분의 경우에 Shape를 만들 때마다 처리해야 하는 특정 객체 생성 매개변수가 있을 것이다.

결과적으로 객체 생성 방식을 변경할 때마다 Shape 객체가 인스턴스화되는 모든 위치에서 코드를 변경해야 한다.

한 곳을 변경하면 잠재적으로 다른 많은 곳에서 코드를 변경해야 하므로 코드가 밀접하게 묶이게 된다..

이 접근법의 또 다른 문제점은 클래스를 사용해 **프로그래머에게 객체 작성 로직을 노출**시킨다는 것이다.

이러한 상황을 해결하기 위해 팩토리 메서드 패턴을 구현할 수 있다.

*[팩토리 메서드 패턴 정리 글](https://fkdl0048.github.io/patterns/Patterns_FactoryMethod/)*

팩토리 메서드는 모든 인스턴스화를 캡슐화해 구현 전반에 걸쳐 균일해야 한다.

팰토리를 사용해 인스턴스화하면, 팩토리는 적절하게 인스턴스화 한다.

##### 팩토리 메서드 패턴

팩토리 메서드 패턴의 근본적인 목적은 정확한 클래스를 지정하지 않고도 객체를 생성하는 일과 사실상 인터페이스를 사용해 새로운 객체 유형을 생성하는 일을 담당하는 것이다.

어떤 면에서는 팩토리를 래퍼라고 생각할 수 있다.(그렇게 생각했었다..)  

객체를 인스턴스화하는 데 중요한 로직이 있을 수 있으며, 우리는 프로그래머(사용자)가 이 로직에 관심을 갖지 않았으면 한다면..

즉, 값을 검색하는 로직이 일부 로직 내부에 있을 때 접근자 메서드의 개념과 거의 같다.(게터 세터)

필요한 특정 클래스를 미리 알 수 없을 때 팩토리 메서드를 사용하면 된다.

즉, 이번 예제의 모든 클래스는 Shape의 서브클래스여야 한다.

사실 팩토리 메서드는 필요한걸 정확하게 모를 때 사용된다.

나중에 클래스의 일부를 추가할 수 있게 되며 필요한 게 무엇인지를 알고 있다면 생성자나 세터 메서드를 통해 인스턴스를 주입할 수 있다.

기본적으로 이게 다형성에 대한 정의이다.

위 링크에도 피자로 같은 예제가 있지만 책의 예제로 한번 더 복습한다.

enum형태로 도형의 종류를 정의한다.

```java
enum ShapeType{
    CIRCLE,
    RECTANGLE,
    TRIANGLE
}
```

생성자와 generate()라고 부르는 추상 메서드만으로 Shape 클래스를 추상적인 것으로 정의할 수 있다.

```java
abstract class Shape{
  private ShapeType shapeType = null;

  public Shape(ShapeType shapeType){
    this.shapeType = shapeType;
  }

  public abstract void generate();
}
```

Circle, Rectangle, Triangle 클래스는 Shape 클래스를 상속받아 구현한다.

```java
class Circle extends Shape{
  public Circle(){
    super(ShapeType.CIRCLE);
    generate();
  }

  @Override
  public void generate(){
    System.out.println("Circle");
  }
}

class Rectangle extends Shape{
  public Rectangle(){
    super(ShapeType.RECTANGLE);
    generate();
  }

  @Override
  public void generate(){
    System.out.println("Rectangle");
  }
}

class Triangle extends Shape{
  public Triangle(){
    super(ShapeType.TRIANGLE);
    generate();
  }

  @Override
  public void generate(){
    System.out.println("Triangle");
  }
}
```

이름에서 알 수 있듯이 ShapeFactory 클래스는 실제적인 팩토리다.

generate()메서드에 초점이 맞춰져 있다.

팩토리는 많은 장점을 제공하지만, generate()메서드는 실제로 Shape를 인스턴스화하는 애플리케이션 내의 유일한 위치이다.

```java
class ShapeFactory{
  public static Shape generateShape(ShapeType shapeType){
    Shape shape = null;

    switch(shapeType){
      case CIRCLE:
        shape = new Circle();
        break;
      case RECTANGLE:
        shape = new Rectangle();
        break;
      case TRIANGLE:
        shape = new Triangle();
        break;
      default:
        // 예외
        break;
    }

    return shape;
  }
}
```

```
생각

모든 디자인 패턴은 핵심적인 부분은 비슷하지만 각각의 패턴도 구현하는 언어, 스타일, 컨벤션의 차이가 다 다르다.

자바라서 그런지 모르겠지만 default보다 차라리 예외를 아래서 잡고 각각 리턴을 하거나, Shape 자체에 static메서드로 두거나, 인터페이스 DI로 좀 더 유연성을 강화할 수도 있다.

개인의 구현 차이와 해당 프로젝트의 규모, 팀 컨벤션을 고려해서 작성하는 것이 바람직한 것 같다.
```

이러한 개별 객체를 인스턴스화하는 전통적인 접근 방식은 프로그래머가 다음과 같이 new 키워드를 사용해 객체를 직접 인스턴스화 하는 것이다.

```java
public class TestFactoryPattern{
  public static void main(String[] args){
    Shape circle = new Circle();
    Shape rectangle = new Rectangle();
    Shape triangle = new Triangle();
  }
}
```

그러나 팩토리 메서드 패턴을 사용하면 다음과 같이 객체를 인스턴스화 할 수 있다.

```java
public class TestFactoryPattern{
  public static void main(String[] args){
    Shape circle = ShapeFactory.generateShape(ShapeType.CIRCLE);
    Shape rectangle = ShapeFactory.generateShape(ShapeType.RECTANGLE);
    Shape triangle = ShapeFactory.generateShape(ShapeType.TRIANGLE);
  }
}
```

더 나아가서 `C#`의 리플렉션을 사용하면 팩토리 메서드 패턴을 사용하지 않고도 객체를 인스턴스화 할 수 있다.

```csharp
public class TestFactoryPattern{
  public static void main(String[] args){
    Shape circle = (Shape)Activator.CreateInstance(typeof(Circle));
    Shape rectangle = (Shape)Activator.CreateInstance(typeof(Rectangle));
    Shape triangle = (Shape)Activator.CreateInstance(typeof(Triangle));
  }
}
```

하지만 같은 추상화 레벨이나 생성 로직의 통일성 개인성을 보장하지 못하기 때문에 접근이나 가독성이 부족할 수 있는 것 같다..

실제로는 데이터 베이스상의 Unit을 읽어와서 객체를 동적으로 생성하여 관리하는 것 같다.

#### 10.3.2.구조패턴

구조 패턴(structural pattern)은 객체 그룹에서 더 큰 구조를 만드는 데 사용된다.

다음 일곱 가지 디자인 패턴이 구조 패턴의 범주에 속한다.

- 어댑터(Adapter, 즉 적응자)
- 브리지(Bridge, 즉 가교)
- 컴포지션(Composition, 즉 '합성체')
- 데코레이터(Decorator, 즉 '장식자')
- 파사드(Facade)
- 플라이웨이트(Flyweight)
- 프록시(Proxy)

##### 어댑터 디자인 패턴

어댑터 패턴은 이미 존재하는 클래스에 대해 다른 인터페이스를 작성하는 방법이다.

어댑터 패턴은 기본적으로 클래스 래퍼를 제공한다.

다시 말해, 기존 클래스의 기능을 새로우면서도 이상적으로 볼 때 더 나은 인터페이스로 통합하는(둘러싸는) 새로운 클래스를 만든다.

래퍼의 간단한 예는 자바 클래스인 Integer다.

Integer 클래스는 그 안에 단일 Integer값을 둘러싼다.

객체지향 시스템에서는 모든 것이 객체이기 때문에, 기본적인 int, float등과 같은 기본 데이터 형식은 객체가 아니다.

이러한 기본 데이터 형식들을 바탕으로 함수를 실행해야 할 때 이런 기본 데이터 형식들을 객체로 취급해야 한다.

따라서 래퍼 객체를 작성하고 그 안에 기본 데이터 형식들을 두고 둘러싼다.

다음과 같은 기본 데이터 형식을 사용할 수 있다.

```java
int myInt = 10;
```

그리고 이 기본 데이터 형식을 Integer로 둘러쌀 수 있다.

```java
Integer myInteger = new Integer(myInt);
```

이제 형식 변환을 수행할 수 있으며, 해당 자료를 문자열로 취급할 수 있다.

```java
String myString = myInteger.toString();
```

이 래퍼를 사용하면 원래부터 있던 정수를 객체로 취급해 객체의 모든 장점을 제공할 수 있다.

이것이 어댑터 패턴의 기본 아이디어이다.

어댑터의 말 그대로 한국의 콘센와 일본의 콘센트를 연결하는 어댑터의 역할을 한다.

사용하는 패키지나 라이브러리의 호환성이나 중간 역할을 위해 래퍼로 감싸는 것이다.

#### 10.3.3.행위패턴

행위 패턴(behavioral pattern)은 다음과 같은 범주로 구성된다.

- 책임 연쇄(Chain of responsibility)
- 커맨드(Command, 즉 '명령') 패턴
- 인터프리터(Interpreter, 즉 '해설자') 패턴
- 이터레이터(Iterator, 즉 '반복자') 패턴
- 미디에이터(Mediator, 즉 '중재자') 패턴
- 메멘토(Memento, 즉 '기념비') 패턴
- 옵저버(Observer, 즉 '관찰자') 패턴
- 스테이트(State, 즉 '상태') 패턴
- 스트레이지(Strategy, 즉 '전략') 패턴
- 탬플릿 메서드(Template method) 패턴
- 비지터(Visitor, 즉 '방문자') 패턴

##### 이터레이터 디자인 패턴

이터레이터(iterator)는 벡터와 같은 컬렉션을 순회하기 위한 표준 메커니즘을 제공한다.

컬렉션의 각 항목 한 번에 하나씩 접근할 수 있도록 기능을 제공해야 한다.

이터레이터 패턴은 정보 은닉을 제공해 컬렉션의 내부 구조를 안전하게 유지한다.

이터레이터 패턴은 또한 서로 간섭하지 않고 둘 이상의 이터레이터가 작성될 수 있도록 규정한다.

`C#`은 `IEnumerable` 인터페이스를 사용해 이터레이터 패턴을 구현한다.

```csharp
public interface IEnumerable{
  IEnumerator GetEnumerator();
}
```

`IEnumerator` 인터페이스는 컬렉션의 각 항목을 순회하는 데 사용된다.

```csharp
public interface IEnumerator{
  bool MoveNext();
  object Current{get;}
  void Reset();
}
```

자바는 다음과 같다.

```java
package Iterator;

import java.util.*

public class Iterator{
  public static void main(String[] args){
    ArrayList<String> list = new ArrayList<String>();

    list.add("one");
    list.add("two");
    list.add("three");
    list.add("four");

    iterate(list);
  }

  public static void iterate(ArrayList<String> list){
    foreach(String s : list){
      System.out.println(s);
    }
  }
}
```

#### 10.3.4. 안티패턴

디자인 패턴은 긍정적인 방식으로 발전하지만, 안티패턴(antipatterns)은 끔찍한 경험들을 모아 놓은 것으로 생각할 수 있다.

안티패턴이란 용어는 특정 유형의 문제를 사전에 해결하기 위해 디자인 패턴이 생성된다는 사실에서 비롯된다.

반면에, 안티패턴은 문제에 대한 반응이고 나쁜 경험으로부터 얻어진다.

요컨대, 디자인 패턴이 견고한 설계 실습을 기반으로 한 반면에, 안티패턴은 피해야할 관행으로 생각할 수 있다.

많은 사람들이 안티패턴이 디자인 패턴보다 더 유용하다고 생각한다.

안티패턴은 이미 발생한 문제를 해결하도록 설계되었기 때문이다..

안티패턴은 기존 설계를 수정하고 실행 가능한 솔루션을 찾을 때까지 해당 설계를 지속적으로 리팩토링한다.

- 안티패턴의 좋은 예 몇가지
  - 싱글톤(Singleton)
  - 서비스 로케이터(Service Locator)
  - 매직 스트링/넘버(Magic String/Number)
  - 인터페이스 부풀리기(Interface Bloat)
  - 예외에 기반한 코딩(Exception Driven Coding)
  - 오류 숨기기/오류 삼키기(Hiding Swallowing Errors)

### 10.4. 결론

패턴은 일상 생활의 일부이며, 이는 객체지향 설계에 대해 생각해야 하는 방식이다.

정보 기술과 관련된 많은 것들과 마찬가지로 해법의 근원은 **실제 상황에서 발각**된다.
