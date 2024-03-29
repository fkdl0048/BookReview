# 아이템 10: 베이스 클래스가 업그레이드된 경우에만 new 한정자를 사용하라

베이스 클래스에서 virtual로 선언하지 않은 멤버를 재정의하려는 경우 new한정자를 사용할 수 있다.

하지만 사용할 수 있다는 것과 잘 사용한다는 것은 매우 다른 문제이다.

왜냐하면 virtual로 선언되지 않은 **메서드를 재정의하면 메서드의 동작 방식을 모호하게 만들 우려**가 있기 때문이다.

베이스 클래스의 메서드와 하위 클래스에서 재정의한 메서드가 완전히 다른 내용을 구현했다 하더라도 이를 사용하는 대부분의 개발자는 두 메서드가 완전히 동일한 작업을 수행할 것으로 기대한다.

```csharp
public class Base
{
    public void DoWork() { Console.WriteLine("Base.DoWork"); }
}

public class Derived : Base
{
    public new void DoWork() { Console.WriteLine("Derived.DoWork"); }
}

public class Program
{
    public static void Main()
    {
        Base b = new Base();
        b.DoWork(); // Base.DoWork

        Derived d = new Derived();
        d.DoWork(); // Derived.DoWork

        Base bd = new Derived();
        bd.DoWork(); // Base.DoWork
    }
}
```

가장 문제가 될 수 있는 예제이다.

우리가 쉽게 사용하는 virtual/override 키워드를 사용하지 않고 new 한정자를 사용하면서 발생하는 문제이다.

의도한 부분이라도 오용될 수 있는 부분, 이름이 같기 때문에, 세부사항이라 문제가 된다.

사용자는 메서드를 호출할 때 사용한 참조나 레이블을 변경한다고 해서 동작이 바뀔 것이라고 생각하지 않기 때문이다.

좀 더 자세하게 본다면 new 한정자는 비가상 메서드를 가상 메서드로 만드는 것이 아니라 클래스의 명명 범위내에 새로운 메서드를 추가하는 것이다.

비가상 메서드는 정적으로 바인딩되므로 호출하는 코드는 정확히 메서드를 호출한다.

반면, 가상 메서드는 동적으로 바인딩되므로 헌타임에 객체의 타입이 무엇이냐에 따라 그에 부합하는 메서드를 호출한다.

따라서 비가상 메서드를 재정의하는 new한정자를 사용하지 않기 위해 베이스 클래스의 모든 메서드를 가상 메서드로 변경하는 것은 말이 안된다.

라이브러리 설계자가 특정 메서드를 가상으로 선언한다는 것은 이 라이브러리를 사용할 사용자와 나름의 **약속**을 한 것과 마찬가지다.

즉, 파생 클래스에서 이 가상 메서드의 구현부를 변경할 것임을 예상하고 있으며, 파생 클래스에서 가상 메서드의 동작 방식을 변경해도 아무런 문제가 없이 수행할 것을 보장하는 것이다.

우리는 다형성이 필요한 메서드나 속성이 무엇인지를 우선 생각해보고 반드시 다형성이 필요한 경우에만 가상 메서드를 사용해야 한다.

new 한정자를 활용해도 좋은 경우는 베이스 클래스에서 이미 사용하고 있는 메서드를 재정의하여 완전히 새로운 베이스 클래스를 만들어야 하는 경우 정도이다.

이미 널리 사용되고 있는 메서드가 있어서 이를 사용하는 코드를 일일이 찾아서 수정하기 어렵거나 혹은 외부 어셈블리에서 이 메서드를 사용하고 있어서 코드를 수정할 수 없는 경우라면 new한정자를 사용해볼 만하다.

하지만 이 경우에서도 책에선 메서드의 이름을 변경하는게 장기적, 원론적으로 더 좋다는 입장이다.

실제로 stackoverflow에서도 new 한정자에 대한 의견은 가독성을 위한 측면 그 이상 이하도 아니라고 한다.

따라서 new한정자를 사용할 때는 각별한 주의가 필요하다.

별 생각 없이 new한정자를 남발하면 메서드를 호출할 때 상당히 모호한 상황이 발생할 수 있다.

베이스 클래스가 업그레이드 되면서 메서드의 이름이 충돌하는 경우는 매우 특별한 경우라서 new 한정자를 검토할 수 있지만 이 경우에 대해서도 신중하게 생각해야 한다.

그 외의 경우라면 new한정자는 사용해서는 안된다.
