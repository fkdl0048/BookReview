## 12. 객체지향 설계의 SOLID 원칙

객체지향 프로그래밍과 관련해 많은 개발자가 가장 흔히 하는 말 중에 하나는 '실제 세계를 모델링할 수 있는 게 객체지향의 주요 장점이다.'라는 말이다.

고전적인 객체지향 개념에서는 많이 사용되는 말이지만, 로버트 마틴씨의 말에 따르면 객체지향이 우리가 생각하는 방식과 밀접하다고 말하는 것은 그저 마케팅에 불과하다고 한다.

그 대신에, 그는 객체지향이란 핵심 의존성들을 역전시킴으로써 경직된 코드나 취약한 코드 및 재사용이 불가능 코드가 되지 않게 하는 식으로 의존체들을 관리하는 일이라고 말한다.

고전적인 객체지향 프로그래밍 과정에서는 코드를 종종 실제 상황에 맞게 모델링하는 경우가 많다.

예를 들어, 개가 포유류의 일종이라면(is-a)이 관계를 상속으로 나타내는게 명백한 선택지이기는 하다.

엄격한 has-a 및 is-a 리트머스 테스트라는 게 수년간 객체지향적 마음가짐의 한 부분을 차지했다.

그러나 이 책 전체에서 보여주듯 상속 관계를 강요하려고 하면 설계 문제가 생길 수 있다.

짖지 않는 개와 짖는 개, 날지 못하는 새와 날 수 있는 새를 상속 설계를 잘 선택해서 하는 것만으로 서로 구분되게 할 수 있다고 생각하는가?

has-a와 is-a를 엄밀하게 구분해 결정하는 데 집중하는 것만이 최선의 접근 방식은 아닐 것이다.

우리는 클래스를 분리하는 데 더 집중해야 한다.

밥 삼촌(로버트 마틴)은 재사용할 수 없는 코드를 설명하기 위해 다음과 같은 세 가지 용어를 정의한다.

- 경직성(rigitity): 프로그램의 한 부분을 변경하면 다른 부분까지 변경해야 하는 경우
- 취약성(fragility): 관련이 없는 곳에서 오류가 발생하는 경우
- 부동성(immobility): 코드를 원래 맥락에서 벗어나 재사용할 수 없는 경우

이러한 문제들을 해결하고 목표를 달성하기 위해 SOLID가 도입되었다.

로버트 마틴이 '**소프트웨어 설계를 더 이해하기 쉽고 더 유연하며 유지보수 가능하게 만들기**'위해 도입한 다섯 가지 설계 원칙을 SOLID라고 한다.

밥 아저씨의 말에 따르면 모든 객체지향 설계에 적용되지만, SOLID원칙은 **애자일** 개발이나 적응형(adaptive) 소프트웨어 개발과 같은 방법론의 핵심 철학을 형성할 수 있다.

- SRP: 단일 책임 원칙(Single Responsibility Principle)
- OCP: 개방-폐쇄 원칙(Open-Closed Principle)
- LSP: 리스코프 치환 원칙(Liskov Substitution Principle)
- ISP: 인터페이스 분리 원칙(Interface Segregation Principle)
- DIP: 의존성 역전 원칙(Dependency Inversion Principle)

이 원칙들을 다뤄보면서 수십년 동안 존재해 온 고전적인 객체지향 원칙과 관련지어 본다.

### 12.1 객체지향 설계의 SOLID 원칙

#### 12.1.1 단일 책임 원칙(SRP)

단일 책임 원칙(Single Responsibility Principle)에 따르면 클래스를 변경한 이유가 단일해야 한다.

프로그램의 각 클래스와 모듈은 단일 작업에 중점을 두어야 한다.

따라서 같은 클래스 안에 다른 이유 때문에 변경될 메서드를 넣지 않도록 한다.

클래스를 설명하는 글에 '그리고'라는 단어가 포함되면 SRP가 깨질 수 있다.

다시 말해서, 모든 모듈이나 클래스는 소프트웨어가 제공하는 기능의 단일 부분에 대해서만 책임을 져야 한다.

그 책임을 완전히 캡슐화해야 한다.

이 추상 클래스를 상속 받은 클래스는 반드시 calcArea() 메서드를 구현해야 한다.

```java
abstract class Shape{
    protected String name;
    protected double area;
    public abstract double calcArea();
}
```

추상 클래스를 상속받은 Circle 클래스는 자체적으로 calcArea() 메서드를 구현한다.

```java
class Circle extends Shape{
    private double radius;
    public Circle(double radius){
        this.radius = radius;
    }
    public double calcArea(){
        area = Math.PI * radius * radius;
        return area;
    }
}
```

> **주의**  
> 단일 책임 원칙에 초점을 맞출 뿐만 아니라 가능한 한 간단한 예제가 되게 하려고 Circle 클래스를 만들었다.

CalculaterAreas라는 클래스는 Shape 배열에 포함된 다른 도형의 면적을 합산한다.

```java
class CalculaterAreas{
    Shape[] shapes;
    double sumTotal = 0;

    public CalculaterAreas(Shape[] shapes){
        this.shapes = shapes;
    }

    public double sumAreas(){
        sumTotal = 0;
        for (Shape shape : shapes){
            sumTotal += shape.calcArea();
        }
        return sumTotal;
    }

    public void output(){
        System.out.println("Sum of the areas of provided shapes: " + sumTotal);
    }
}
```

CalculaterAreas 클래스는 출력을 처리하므로 문제가 있다.

면적 계산 행위와 출력 행위가 같은 클래스에 포함되어 있다.

```java
public class TestShape{
    public static void main(String[] args){
        Circle circle = new Circle(1);

        Shape[] shapeArray = new Shape[1];
        shapeArray[0] = circle;

        CalculaterAreas calculator = new CalculaterAreas(shapeArray);

        calculator.sumAreas();
        calculator.output();
    }
}
```

이 처럼 테스트 애플리케이션이 준비되면 단일 책임 원칙 문제에 중점을 둘 수 있다.

다시 말하지만 이 클래스는 출력 행위와 합산 행위가 포함되어 있다.

여기서 근본적인 문제는 다음과 같다.

메서드의 기능을 변경하려면 면적을 합산하는 메서드를 변경할지 여부에 관계없이 CalculatorAreas 클래스를 변경해야 한다.

**예를 들어, 어떤 시점에서 간단한 텍스트가 아닌 HTML로 출력하고 싶다면 어떻게 해야 할까?**

지금은 책임이 결합되어 있기 때문에 영역을 합한 코드를 다시 컴파일하고 재배치해야 한다.

단일 책임 원칙에 따르면, 한 메서드를 변경해도 다른 메서드에 영향을 미치지 않게 하여 불필요한 컴파일이 없도록 하는 것이 목표다.

> '한 클래스에는 변화해야 할 이유가 한 가지, 아니 단 한 가지여야 한다. 즉, 변화해야 할 책임이 단일해야 한다.'

이를 해결하기 위해, 두 개의 메서드를 서로 분리된 클래스에 넣을 수 있는데, 그중에 하나는 원래 콘솔 출력용이고 다른 하나는 HTML 출력용이다.

```java
class CalculaterAreas{
    Shape[] shapes;
    double sumTotal = 0;

    public CalculaterAreas(Shape[] shapes){
        this.shapes = shapes;
    }

    public double sumAreas(){
        sumTotal = 0;
        for (Shape shape : shapes){
            sumTotal += shape.calcArea();
        }
        return sumTotal;
    }
}

class OutputAreas{
    double areas = 0;

    public OutputAreas(double areas){
        this.areas = areas;
    }

    public void console(){
        System.out.println("Sum of the areas of provided shapes: " + areas);
    }

    public void html(){
        System.out.println("<HTML>");
        System.out.println("Sum of the areas of provided shapes: " + areas );
        System.out.println("</HTML>");
    }
}
```

이제 테스트 애플리케이션을 다시 작성해보자.

```java
public class TestShape{
    public static void main(String[] args){
        Circle circle = new Circle(1);

        Shape[] shapeArray = new Shape[1];
        shapeArray[0] = circle;

        CalculaterAreas calculator = new CalculaterAreas(shapeArray);
        OutputAreas output = new OutputAreas(calculator.sumAreas());
        
        output.console();
        output.html();
    }
}
```

여기서 요점은 요구사항에 따라 다양한 대상으로 출력을 보낼 수 있다는 것이다.  

그 밖에도 Json이 필요하다면 CalculatorAreas 클래스를 변경하지 않고 OutputAreas 클래스에 Json 출력 메서드를 추가할 수 있다.

결과적으로 CalculatorAreas클래스를 독립적으로 재배포하면서도 그 밖의 클래스에 전혀 영향을 미치지 않는다.

```
생각

개인적인 생각으로 OutputAreas 클래스는 한 가지 행위만 하고 있긴 하지만 좀 더 나아가서 콘솔 출력과 HTML 출력을 각각 클래스로 분리하는 것이 더 좋을 것 같다.

인터페이스로 묶어서 해당 인터페이스를 상속받아 출력 기능을 각각 구현하는 것이 좀 더 바람직..?

클래스를 쪼갤 수 있으면 쪼개서 작게 만들어야 하기도 하고 DI를 사용해 사용자에게 new가 아닌 주입을 받아 사용하는 것이 좋다고 생각한다.
```

#### 12.1.2. 개방/폐쇄 원칙(OCP)

개방/폐쇄 원칙(Open/Close Principle)에 따르면 클래스를 수정하지 않고 클래스의 행위를 확장할 수 있어야 한다.

```java
class Rectangle{
    public double length;
    public double width;

    public Rectangle(double length, double width){
        this.length = length;
        this.width = width;
    }
}

class CalculateAreas{
    private double area;

    public double calculateRectangleArea(Rectangle rectangle){
        area = rectangle.length * rectangle.width;
        return area;
    }
}

public class OpenColsed{
    public static void main(String[] args){

        Rectangle rectangle = new Rectangle(1, 2);
        CalculateAreas calculator = new CalculateAreas();

        System.out.println("Area of rectangle: " + calculator.calculateRectangleArea(rectangle));
    }
}
```

이 애플리케이션이 사각형에 한해서 작동한다는 사실은 개방/폐쇄 원칙을 설명하는 제약 조건을 제공한다.

CalculateAreas 클래스에 Circle을 추가하려면 모듈 자체를 변경해야 한다.

분명히 이것은 개방/폐쇄 원칙과 상충되며, 모듈을 변경하기 위해 모듈을 변경할 필요가 없다는 것을 명시하고 있다.

Shape라는 추상 클래스를 만들어 상속받게 하여 getArea()의 구현을 강제할 수 있는데, 이때 Shape클래스 자체를 변경할 필요 없이 원하는 만큼의 다양한 클래스를 추가할 수 있다.

이제 Shape클래스가 폐쇄되어 있다고 말할 수 있다.

```java
abstract class Shape{
    public abstract double getArea();
}

class Rectangle extends Shape{
    public double length;
    public double width;

    public Rectangle(double length, double width){
        this.length = length;
        this.width = width;
    }

    @Override
    public double getArea(){
        return length * width;
    }
}

class Circle extends Shape{
    public double radius;

    public Circle(double radius){
        this.radius = radius;
    }

    @Override
    public double getArea(){
        return Math.PI * radius * radius;
    }
}

class CalculateAreas{
    private double area;

    public double calculateArea(Shape shape){
        area = shape.getArea();
        return area;
    }
}

public class OpenColsed{
    public static void main(String[] args){

        Shape rectangle = new Rectangle(1, 2);
        Shape circle = new Circle(1);

        CalculateAreas calculator = new CalculateAreas();

        System.out.println("Area of rectangle: " + calculator.calculateArea(rectangle));
        System.out.println("Area of circle: " + calculator.calculateArea(circle));
    }
}
```

이런 식으로 구현하면 새 Shape를 추가할 때 CalculateAreas 클래스를 변경할 필요가 없다.

이는 레거시 코드에 대한 걱정없이 코드를 확장할 수 있음을 의미한다.

핵심은 개방/폐쇄 원리는 자식 클래스를 통해 코드를 확장해야 하며, 원래 클래스는 변경할 필요가 없다는 것이다.

그러나 확장(Extension)이라는 단어는 SOLID와 관련된 여러 토론에서 문제가 된다.

우리가 상속보다 합성을 선호한다면 이것이 개방/폐쇄 원칙에 어떤 영향을 미칠까?

SOLID 원칙 중 하나를 따르는 경우에, 코드가 또 다른 SOLID 원칙 중 하나를 따라야 할 수 있다.

예를 들어, 개방/폐쇄 원칙을 따르도록 코드를 설계했는데 이 코드가 단일 책임 원칙을 준수할 수도 있다는 말이다..!

```
생각

단일 책임 원칙에서는 Shape를 가지고 다형성을 구현, 여기서도 Shape를 가지고 다형성을 구현했다.

다른 점은 CalculateAreas 클래스의 행위에 대한 인터페이스 설계인 것 같다.

해당 인터페이스를 열어 두어 다형성, 캡슐화를 위해 클래스를 분리하고 이를 보여주는 예제가 아마 나올 듯 하다.
```

#### 12.1.3. 리스코프 대체 원칙(LSP)

리스코프 대체(치환) 원칙(Liskov Substitution Principle)에 따르면 부모 클래스의 인스턴스를 해당 자식 클래스 중 하나의 인스턴스로 교체할 수 있게 설계해야 한다.

부모 클래스가 무언가를 할 수 있다면 자식 클래스도 그것을 할 수 있어야 한다.

합리적으로 보일 수 있지만.. 리스코프 대체 원칙을 위반하는 사례를 먼저 본다.

```java
abstract class Shape{
    protected double area;

    public abstract double getArea();
}

class Rectangle extends Shape{
    private double length;
    private double width;
    
    public Rectangle(double length, double width){
        this.length = length;
        this.width = width;
    }

    @Override
    public double getArea(){
        area = length * width;
        return area;
    }
}

class Square extends Rectangle{
    public Square(double side){
        super(side, side);
    }
}

public class LiskovSubstitution{
    public static void main(String[] args){
        
        Rectangle rectangle = new Rectangle(1, 2);

        System.out.println("Area of rectangle: " + rectangle.getArea());

        Square square = new Square(2);

        System.out.println("Area of square: " + square.getArea());
    }
}
```

문제없는 코드이다.

직사각형은 도형의 일종으므로(is-a) 모든 것이 좋아보인다.

정사각형또한 직사각형의 일종으로(is-a) 아직까지는 문제가 없는 것 같다.

만약 이런 관계가 아니라면 어떻게 해야 할까?

이제는 다소 철학적인 이야기를 해야한다.

정사각형이 실제로는 직사각형의 일종인가?

많은 사람들이 그렇게 말하지만 정사각형은 특수한 유형의 도형일 수 있지만 직사각형과는 속성이 다르다.

직사각형은 사격형이기도 하지만 평행사변형이기도 하다.(대각에 놓인 변끼리 일치하는 경우)

한편, 정사각형은 마름모이지만, 직사각형은 그렇지 않다.

따라서 직사각형과 정사각형 간에는 약간 다른 점이 있다.

실제로는 객체지향 설계 시에 기하학이 문제 되지는 않는다.

우리가 직사각형과 정사각형을 어떻게 만드느냐가 문제다.

```java
public Rectangle(double l, double w){
    length = l;
    width = w;
}
```

생성자에는 분명히 두 개의 매개변수가 필요하다.

그러나 부모 클래스인 Rectangle은 Square생성자가 두 개이기를 기대하지만, 한 개만 필요하다.

실제로 면적을 계산하는 기능은 두 클래스에서 미묘하게 다르다.

사실 Square는 동일한 매개변수를 두 번 전달하여 Rectangle을 속인다.

이것은 받아들일 수 있는 해결책처럼 보일지 모르지만,**실제로는 코드를 혼란스럽게 할 수 있고 의도하지 않은 유지보수 문제를 초래할 수 있다.**

이것은 일관성이 없는 설계 결정이며, 아마도 의심스러운 설계 결정일 것이다.

생성자가 다른 생성자를 호출하는 것을 보게 된다면 설계를 일시 중지하고 다시 생각해보는 게 좋다.

*적절하지 않은 자식 클래스가 아니여서 그럴 수 있기 때문*

이런 딜레마를 해결하기 위해선 정사각형과 직사각형을 다음과 같이 설계해야 한다.

```java
abstract class Shape{
    protected double area;

    public abstract double getArea();
}

class Rectangle extends Shape{
    private double length;
    private double width;
    
    public Rectangle(double length, double width){
        this.length = length;
        this.width = width;
    }

    @Override
    public double getArea(){
        area = length * width;
        return area;
    }
}

class Square extends Shape{
    private double side;

    public Square(double side){
        this.side = side;
    }

    @Override
    public double getArea(){
        area = side * side;
        return area;
    }
}

public class LiskovSubstitution{
    public static void main(String[] args){
        
        Rectangle rectangle = new Rectangle(1, 2);

        System.out.println("Area of rectangle: " + rectangle.getArea());

        Square square = new Square(2);

        System.out.println("Area of square: " + square.getArea());
    }
}
```

#### 12.1.4. 인터페이스 분리 원칙(ISP)

인터페이스 분리 원칙(Interface Segregation Principle)에 따르면 몇 개의 큰 인터페이스가 있는 편보다는 작은 인터페이스가 많은 편이 바람직하다.

*클래스도 마찬가지*

이번 예제는 Mammal, eat() 및 makeNoise()에 대한 여러 행위를 포함하는 단일 인터페이스를 작성한다.

```java
interface Mammal{
    public void eat();
    public void makeNoise();
}

class Dog implements Mammal{
    @Override
    public void eat(){
        System.out.println("Dog eats");
    }

    @Override
    public void makeNoise(){
        System.out.println("Dog barks");
    }
}

public class myClass{
    public static void main(String[] args){
        Dog dog = new Dog();
        dog.eat();
        dog.makeNoise();
    }
}
```

이것은 잘 작동하지만, Mammal 인터페이스는 Dog 클래스에 대해 너무 많은 것을 요구한다.

Mammal에 대한 단일 인터페이스를 만드는 대신에 모든 행위에 대해 별도의 인터페이스를 만들 수 있다.

```java
interface Eater{
    public void eat();
}

interface NoiseMaker{
    public void makeNoise();
}

class Dog implements Eater, NoiseMaker{
    @Override
    public void eat(){
        System.out.println("Dog eats");
    }

    @Override
    public void makeNoise(){
        System.out.println("Dog barks");
    }
}

public class myClass{
    public static void main(String[] args){
        Dog dog = new Dog();
        dog.eat();
        dog.makeNoise();
    }
}
```

실제로 우리는 Mammal 클래스에서 **행위를 분리**한다.  

따라서 상속을 통해 단일 Mammal 엔터티를 만드는 대신에 이전 장에서 취한 전략과 비슷한 **합성 기반 설계**로 이동한다.

*즉, 이 원칙을 사용하면 자연스럽게 합성 기반으로 설계가 이동된다.*

이 접근법을 사용하면 단일 Mammal 클래스에 포함된 행위를 강요하지 않고 합성으로 Mammal들을 만들 수 있다.

예를 들어 누군가가 먹지 않고 대신에 피부를 통해 영양분을 흡수하는 Mammal을 발견했다고 가정해보자.

eat()이라는 행위가 포함된 단일 Mammal 클래스에서 상속받으면 새 포유류에는 이 행위가 필요하지 않다.

그러나 모든 행위를 별도의 단일 인터페이스로 분리하면 각 포유동물을 정확하게 제시하는 방식으로 구축할 수 있다.

#### 12.1.5. 의존성 역전 원칙(DIP)

의존성 역전 원칙(Dependency Inversion Principle)은 코드가 추상화에 의존해야 한다고 명시하고 있다.

종종 의존성 역전과 의존성 주입이라는 용어가 서로 교환해서 쓸 수 있는 말처럼 들리겠지만, 이 원칙을 논의할 때 이해해야 할 몇 가지 핵심 용어는 다음과 같다.

- **의존성 역전**: 의존체들을 역전시키는 **원칙**
- **의존성 주입**: 의존체들을 역전시키는 **행위**
- **생성자 주입**: 생성자를 통해서 의존성을 주입
- **파라미터 주입**: 메서드의 파라미터를 통해서 의존성을 주입

의존성 역전의 목표는 구상적인 것에 결합하기보다는 추상적인 것에 결합하는 것이다.

**어떤 시점에서는 분명히 구상적인 것을 만들어야 하지만, 우리는 main() 메서드에서와 같이 가능한 한 사슬을 멀리 뻗어 구상 객체를 만들려고 노력한다.**

```
생각

가능한 한 사슬을 멀리 뻗어 구상 객체를 만들려고 한다..

우리는 최대한 추상적으로 다뤄야 코드의 유연성이 높아진다는 것을 알지만, 그 시점이 어디쯤인지 몰라서 망설이는 경우도 있을 것 이라고 생각한다.

멀리 뻗어 구상 객체를 만들려고 한다는 말 자체가 앞 장을 읽고 경험을 해보니 이해가 되는 말이라 되게 와닿는다.
```

의존성 역전 원칙의 목표 중 하나는 컴파일타임이 아니라 **런타임에 객체를 선택**하는 것이다.

우리는 이전 클래스를 다시 컴파일하지 않고도 새 클래스를 작성할 수도 있다.(새 클래스를 작성해 주입)

##### 1단계: 초기 예제

Mammal클래스의 예제를 다시 살펴본다.

```java
abstract class Mammal{
    public abstract String makeNoise();
}
```

Cat과 같은 자식 클래스는 상속을 사용해 포유류의 행위인 makeNoise()를 활용한다.

```java
class Cat extends Mammal{
    @Override
    public String makeNoise(){
        return "Meow";
    }
}

class Dog extends Mammal{
    @Override
    public String makeNoise(){
        return "Bark";
    }
}
```

```java
public class TestMammal{
    public static void main(String[] args){
        Mammal cat = new Cat();
        Mammal dog = new Dog();

        System.out.println(cat.makeNoise());
        System.out.println(dog.makeNoise());
    }
}
```

##### 2단계: 행위를 분리해 내기

앞의 코드에는 잠재적으로 심각한 결함이 있다.

코드는 포유류와 행위를 연결한다.

포유류의 행위를 포유동물 자체로부터 분리하면 상당한 이점을 얻을 수 있다.

이렇게 하기 위해 우리는 포유류뿐만 아니라 포유류가 아닌 것들도 모두 사용할 수 있는 MakingNoise라는 클래스를 만든다.

```java
abstract class MakingNoise{
    public abstract String makeNoise();
}

class CatNoise extends MakingNoise{
    @Override
    public String makeNoise(){
        return "Meow";
    }
}
```

Cat 클래스와 분리된 MakingNoise 행위를 사용하면 다음 코드 조각과 같이 Cat 클래스 자체의 하드코딩된 행위 대신에 CatNoise 클래스를 사용할 수 있다.

```java
abstract class Mammal{
    public abstract String makeNoise();
}

class Cat extends Mammal{
    private MakingNoise noise = new CatNoise();

    @Override
    public String makeNoise(){
        return noise.makeNoise();
    }
}
```

두 번째 단계까지 적용된 전체 애플리케이션이다.

```java
public class TestMammal{
    public static void main(String[] args){
        Mammal cat = new Cat();
        Mammal dog = new Dog();

        System.out.println(cat.makeNoise());
        System.out.println(dog.makeNoise());
    }
} 
```

문제는 명확하다.  

주요 부분은 분리했지만, Cat은 여전히 Cat울음 소리내기 행위를 인스턴스화 하기 때문에 우리는 의존성 역전이라는 목표에 도달하지 못했다는 것이다.

`private CatNoise noise = new CatNoise();`

Cat은 저수준 모듈인 CatNoise에 결합된다.

다시 말해서 Cat은 CatNoise와 연결되서는 안 되며, 울음 생성을 위한 추상화에 연결되어야 한다.

실제로 Cat 클래스는 울음 생성 동작을 인스턴스화하지 말고 대신 주입을 통해 받아야 한다.

*이는 처음 설명한 구상적인 부분이 아닌 런타임에 추상적으로 처리 가능하기 때문에 Cat이 메인이 아니다.*

##### 3단계: 의존성 주입

이 마지막 단계에서 우리는 설계의 상속 측면을 완전히 버리고 합성을 통한 의존성 주입을 활용하는 방법을 조사한다.

상속보다는 합성이라는 개념이 탄력을 받는 주요 이유 중 하나는 **상속 위계구조가 필요하지 않다는 점**이다.

초기 구현엔 Cat과 Dog가 기본적으로 정확히 같은 코드를 포함하고 있다.

서로 다른 울음소리만 돌려줄 뿐이다.

결과적으로는 상당한 중복이 발생한다.

따라서 포유동물이 많으면 울음소리를 유발하는 코드가 많을 것이다.

아마도 더 나은 설계는 포유류가 울음소리를 내도록 코드를 취하는 것이다.

한 단계 나아가서 특정 포유류를 버리고 다음과 같이 간단히 Mammal클래스를 사용하는 것이다.

```java
class Mammal{
    MakingNoise noise;

    public Mammal(MakingNoise noise){
        this.noise = noise;
    }

    public String makeNoise(){
        return noise.makeNoise();
    }
}
```

이제는 Cat 울음 소리 생성 행위를 인스턴스화하고 이를 Animal 클래스에 제공하여 Cat처럼 행위를 하는 포유류를 만들 수 있다.

실제로, 전통적인 클래스 구축 기술을 사용하는 대신에 행위를 주입하여 언제든 Cat을 조립할 수 있다.

```java
Mammal cat = new Mammal(new CatNoise());
```

이제는 의존성 주입을 논의할 때 **객체를 실제로 인스턴스화하는 시점**이 중요한 고려 사항이다.

목표는 주입을 통해 객체를 작성하는 것이지만, 어느 시점에서는 객체를 인스턴스화해야 한다.

결과적으로 설계 결정은 이 인스턴스화를 수행할 시기를 중심으로 이루어진다.

의존성 역전의 목표는 특정 시점에서 구체적으로 무언가를 만들어야 하지만, 무언가 구상적인 게 아닌 추상적인 것에 결합하는 것이다.

**따라서 간단한 목표 중 하나는(new 키워드를 사용함으로써) 메서드에서 그러는 것처럼 최대한 멀리까지 이어지게 구상 객체를 만드는 것이다.**

new 키워드를 볼 때 마다 언제나 그 대상의 값을 평가하자.

### 12.2. 결론

SOLID원칙은 오는날 사용되는 가장 영향력 있는 객체지향 지침 중 하나이다.

필자는 SOLID의 가장 흥미로운 점을 상투적인 부분이 없다는 것을 강조하는데 각각의 특성이 캡슐화, 상속, 다형성 그리고 합성까지 연결되는 부분에 집중한다.

특히, 상속 대 합성..

개인적인 생각 + 많은 말들이 그러하듯 정말 좋은 원칙이고 따라야 하는 것은 맞지만 신봉하지는 말자 주의이다.

팀 협업에 있어 다른 인원이 이해하기 쉽고 유지보수가 쉬운 코드를 작성하는 것이 더 중요하다고 생각된다.

물론 이런 원칙에 맞춰 짜여진 코드가 읽기 쉽겠지만 다른 사람의 코드가 그러지 못하다 해서 비난하는게 맞을까?

코드리뷰를 통해 교정은 해야하겠지만 모른다고 무시하는 것은 아닌 것 같다.

최근 친구와 대화에서 코틀린 관련 public에 대한 이야기를 나눴는데 친구는 객체지향 빠돌이에 대한 부정적인 인식이 강했다.

개인적으로 객체지향을 공부하는 입장에선 쉽게 이해가지 않았지만 친구의 자바 롬복 라이브러리, 코틀린의 성격에 대한 이야기를 들어보니 그럴 수 있겠다는 생각이 들었다..

너무 개인적인 이야기까지 책 리뷰에 적은 것 같지만 이번 책은 여기서 마무리.
