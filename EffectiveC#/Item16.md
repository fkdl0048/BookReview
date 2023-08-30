# 아이템 16: 생성자 내에서는 절대로 가상 함수를 호출하지 말라

객체가 완전히 생성되기 이전에 가상 함수를 호출하면 이상 동작을 일으킨다.

어떤 타입이든 생성자가 수행을 완료할 때까지는 객체가 완전히 생성되었다고 할 수 없다.

따라서 생성자 내에서 가상 함수를 호출하면 예상처럼 동작하지 않는다.

```cs
class B
{
    protected B()
    {
        VFunc();
    }

    protected virtual void VFunc()
    {
        Console.WriteLine("B.VFunc()");
    }
}

class Derived : B
{
    private readonly string msg = "Set by initializer";

    public Derived(string mag)
    {
        this.msg = msg;
    }

    protected override void VFunc()
    {
        Console.WriteLine(msg);
    }

    public static void Main()
    {
        var d = new Derived("Set by constructor");
    }
}
```

이 코드의 결과는 "Set by initializer"가 출력된다.

베이스 클래스의 생성자를 살펴보면 자기 클래스 내에 정의된 가상 함수를 호출하고 있다.

하지만 파생 클래스가 가상 함수를 이미 재정의하고 있기 때문에 런타임에는 파생 클래스에서 재정의한 함수가 호출된다.

왜냐하면 런타임에 객체의 타입이 Derived로 결정되기 때문이다.

C#의 정의에 따르면 생성자의 본문으로 진입하는 순간 해당 객체는 이미 초기화가 완료된 것으로 간주한다.

모든 멤버 변수를 초기화 구문을 이용해 초기화할 수 있을 지 몰라도, 대부분의 경우 모든 멤버 변수가 이 시점에 유효한 값을 갖도록 초기화되었다고 단정하기는 어렵다.

단지 멤버 변수에 대한 초기화 구문을 완료했을 뿐이며 실상 파생 클래스의 생성자 본문은 아직 수행조차 되지 않았기 때문이다.

이 때문에 객체를 생성하는 동안 가상 함수를 호출하면 일관성 문제가 발생할 수 있다.

C++의 경우 가상 함수가 생성 중인 객체의 타입을 확인하도록 설계되었다.

또한 런타임 객체의 타입은 객체가 완전히 생성된 이후에 확정될 수 있도록 설계되었다.

우선 생성 중인 객체는 Derived 타입의 객체다.

따라서 Derived 객체에서 재정의한 가상 함수를 호출해야 한다.

이 부분에 대한 C++의 규칙은 C#과는 다르다.

먼저 C++의 경우 각 클래스의 생성자가 실행되면 객체의 런타임 타입이 변경된다.

둘째로 C#에서는 현재 타입이 추상 베이스 클래스인 경우 가상 메서드가 null 메서드 포인터가 될 가능성을 원칙적으로 배제하려 했다.

```cs
abstract class B
{
    protected B()
    {
        VFunc();
    }

    protected abstract void VFunc();
}

class Derived : B
{
    private readonly string msg = "Set by initializer";

    public Derived(string mag)
    {
        this.msg = msg;
    }

    protected override void VFunc()
    {
        Console.WriteLine(msg);
    }

    public static void Main()
    {
        var d = new Derived("Set by constructor");
    }
}
```

이 코드의 결과는 "Set by initializer"가 출력된다.

*이런 오용되거나 원자적이지 않은 차이가 매우 중요..*

처음에는 동적 바인딩과 같이 유용하다고 느꼈는데 실제로 실습해보니 왜 사용하지 말아야 하는지 이해가 된다.

이 경우 B타입의 객체를 생성할 수 없기 때문에 파생 클래스에서 반드시 vFunc()를 구현해야 정상적으로 컴파일된다.

C#은 런타임을 고려하여 VFunc()를 호출한다.

**이러한 설계 방법이 생성자 내에서 추상 함수를 호출했을 때 런타임 예외를 피할 수 있는 유일한 해법이기 때문이다.**

*실제로 C++에선 크래시가 발생한다.*

하지만 C#의 이러한 전략은 위험해 보인다.

실제로 msg는 변경이 불가능하도록 readonly 변수로 선언했으며, 객체의 전체 수명 동안 동일한 값을 가져야 한다.

하지만 생성자가 작업을 완료할 때까지 잠깐 동안이지만 msg는 의도하지 않게 다른 값으로 변경된다.

멤버 초기화 구문과 생성자 본문은 하나의 세트로 봐야 한다.

베이스 클래스의 생성자 내에서 가상함수를 호출하면 파생 클래스가 가상 함수를 어떻게 구현했는지에 따라 매우 민감하게 동작한다.

파생 클래스가 어떻게 작성될지 예상할 수는 없는 노릇이므로 베이스 클래스의 생성자 내에서 가상 함수를 호출하게 되면 구조가 매우 취약한 코드가 돼버린다.

파생 클래스에서 멤버 초기화 구문을 통해서 모든 변수를 초기화하면 될 것 같지만 대부분의 경우 생성자로 전달된 매개변수를 이용하여 객체를 초기화할 것이기 때문에 이 같은 방법은 제약이 너무 많다.
