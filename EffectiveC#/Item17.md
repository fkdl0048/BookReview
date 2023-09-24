# 아이템 17: 표준 Dispose 패턴을 구현하라

비관리 리소스를 포함하는 객체는 정리 작업이 매우 중요하다고 앞 서 이야기 한 바 있다.

이제 메모리가 아닌 다른 유형의 비관리 리소스를 포함하는 타입을 작성할 때 리소스 관리를 어떻게 해야 할지를 살펴볼 차례다.

이미 .NET Framework 내부에서는 비관리 리소스를 정리하는 표준화된 패턴을 사용하고 있으므로 새로운 타입을 만들 때도 동일한 패턴을 이용하는 것이 좋다.

이는 `Dispose`패턴으로 알려져 있으며 타입을 작성할 때 이 패턴을 이용하면 개발자들에게 `IDisposable`인터페이스를 통해서 리소스를 삭제할 수 있는 기능을 안정적으로 제공할 수 있다.

게다게 비관리 리소스를 명시적으로 정리해야 한다는 사실을 잊어버리거나 인지하지 못한 경우도 finalizer를 통해 올바르게 리소스가 정리될 수 있도록 해준다.

표준 Dispose 패턴은 가비지 수집기와 연계되어 동작하며 불가피한 경우에만 finalizer를 호출하도록 하여 성능에 미치는 부정적인 영향을 최소화한다.

이 패턴은 비관리 리소스를 다루기 위한 가장 효과적인 방법으로 알려져 있으므로 .NET을 사용하는 개발자라면 반드시 알아야 한다.

> 비관리 리소스에 대한 MS 정리  
> 가장 일반적인 형태의 관리되지 않는 리소스로는 파일, 창, 네트워크 연결 또는 데이터베이스 연결 등의 운영 체제 리소스를 래핑하는 개체를 들 수 있습니다

실제로 .NET에서는 비관리 리소스를 [System.Runtime.Interop.SafeHandle](https://learn.microsoft.com/ko-kr/dotnet/api/system.runtime.interopservices.safehandle?view=net-7.0)을 상속한 파생 클래스를 통해 표현하는데 이 클래스 또한 Dispose패턴을 완벽하게 구현하고 있다.

상속 계통상 최상위의 베이스 클래스는 다음과 같은 작업을 수행해야 한다.

- 리소스를 정리하기 위해서 IDisposable인터페이스를 구현해야 한다.
- 멤버 필드로 비관리 리소스를 포함하는 경우에 한해 방어적으로 동작할 수 있도록 finalizer를 추가해야 한다.
- Dispose와 finalizer는 실제 리소스 정리 작업을 수행하는 다른 가상 메서드에 작업을 위임하도록 작성되어야 한다.
- 파생 클래스가 고유의 리소스 정리 작업이 필요한 경우 이 가상 메서드를 재정의할 수 있도록 하기 위함이다.

파생 클래스는 다음 작업을 수행해야 한다.

- 파생 클래스가 고유의 리소스 정리 작업을 수행해야 한다면 베이스 클래스에서 정의한 가상 메서드를 재정의한다.
- 멤버 필드로 비관리 리소스를 포함하는 경우에만 finalizer를 추가해야 한다.
- 베이스 클래스에서 정의하고 있는 가상 함수를 반드시 재호출해야 한다.

**비관리 리소스를 포함하는 클래스는 반드시 finalizer를 구현해야 한다.**

사용자가 항상 Dispose()메서드를 올바르게 호출할 것이라고 가정할 수 없기 때문이다.

가비지 수집 작업이 수행되면 finalizer가 없는 가비지 객체는 즉각 메모리에서 제거되지만 finalizer를 가진 객체는 여전히 메모리에 남게 된다.

가비지 수집기는 finalizer큐라는 곳에 객체들의 참조를 삽입해두고, finalizer 스레드라는 특별한 스레드를 이용해 이 큐에 포함된 객체들의 finalizer를 순차적으로 호출한다.

finalizer를 호출한 객체들에 대해서는 더 이상 finalizer를 호출할 필요가 없음을 나타내는 플래그를 설정하고 이제 메모리로부터 제거될 수 있는 대상으로 간주한다.

불행한 것은 앞서 finalizer큐에 삽입된 객체들은 이전에 수행된 가비지 수집 작업을 통해서 정리되지 못한 객체이므로 자연스럽게 한 세대가 높아진다.

> 세대 개념이란? 앞 Item: 11 .NET 리소스 관리에 대한 이해 파트에서 볼 수 있다.

이 때문에 다른 객체에 비해서 상대적으로 메모리에 오래 남아있게 된다.

가비지 수집 작업이 수차례 반복되어 해당 세대에 대한 가비지 수집이 수행되면 비로소 이 객체들은 제거될 기회를 얻는다.

## IDisposable 구현

IDisposable을 구현한다는 것은 사용자와 .NET 런타임에게 적시에 리소스를 정리할 수 있는 방법이 있다는 것을 알려주기 위한 표준화된 방법이기도 하다.

```cs
public interface IDisposable
{
    void Dispose();
}
```

IDisposable.Dispose() 메서드는 다음 네 가지 작업을 반드시 수행해야 한다.

- 모든 비관리 리소시를 정리한다.
- 모든 관리 리소스를 정리한다.
- 객체가 이미 정리되었음을 나타내기 위한 상태 플래그 설정. 앞서 이미 정리된 객체에 대하여 추가로 정리 작업이 요청될 경우 이 플래그를 확인하여 `ObjectDisposed` 예외를 발생시킨다.
- finalizer 호출 회피. 이를 위해 GC.SupperssFinalize(this)를 호출한다.

IDisposable은 비관리 리소스를 정리하는 표준화된 방법으로 온전하게 구현했다면 사용자에게 적시에 리소스를 해제할 수 있는 메커니즘을 안전하게 제공한다고 할 수 있다.

*이 정도면 외부에 내놓아도 부끄럽지 않은 타입이라고 할 만하다.*

하지만 여전히 부족한 부분이 존재한다.

첫째, 이 클래스를 상속하여 파생클래스를 정의하는 경우다.

패상 클래스가 자신이 포함하고 있는 리소스를 정리하는 것은 그렇다고 치더라도 베이스 클래스가 포함하고 있는 리소스는 어떻게 정리해야 할까?

이러한 문제점을 해결하려면 파생 클래스가 finalizer나 자신만의 IDisposable을 구현할 때 반드시 베이스 클래스에서 구현한 함수를 호출하도록 작성해야 한다.

이렇게 해야만 베이스 클래스도 올바르게 리소스를 정리할 수 있기 때문이다.

둘째, finalizer와 Dispose메서드는 일반적으로 동일한 역할을 수행하므로 중복된 코드가 여러 번 나타날 수 있다는 점이다.

표준 Dispose 패턴에서 정의하고 있는 세 번째 메서드는 protected로 선언된 가상 헬퍼 함수다.

이 함수의 역할은 리소스 정리를 위한 공통 작업을 수행하고 파생 클래스에게 리소스를 정리할 기회를 주는 것이다.

```cs
protected virtual void Dispose(bool isDisposing);
```

이 가상 함수를 구현해두면 finalizer와 Dispose양쪽에서 사용할 수 있다.

게대가 가상 함수이므로 파생 클래스에서 이 메서드를 재정의하여 자신이 소유한 리소스를 정리하는 코드를 작성할 수 있다.

코드의 마지막 부분에서는 반드시 베이스 클래스에서 정의하고 있는 Dispose(bool)함수를 호출해야 한다.

Dispose(bool)을 호출할 때는 매우 중요한 약속이 있다.

관리 리소스와 비관리 리소스 모두를 제거하려면 isDisposing으로 true로 전달하고 비관리 리소스만 정리하려면 false로 전달해야 한다는 것이다.

다양한 이야기지만 어떤 값을 주더라도 베이스 클래스의 Dispose(bool) 메서드로 진입할 것이므로 베이스 클래스 또한 리소스를 정리할 기회를 갖게 된다.

## 예제

```cs
public class MyResourceHog : IDisposable
{
    // 이미 dispose 되었는지 나타내는 플래그
    private bool alreadyDisposed = false;

    // IDisposable을 구현
    // 가상의 Dispose 메서드를 호출하고
    // finalizer를 회피하도록 한다.
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }
    
    // 가상의 Dispose 메서드
    protected virtual void Dispose(bool isDisposing)
    {
        // Dispose는 한 번만 수행되도록 한다.
        if (alreadyDisposed)
            return;

        if (isDisposing)
        {
            // 여기서 관리 리소스를 정리한다.
        }

        // 여기서 비관리 리소스를 정리한다.

        // disposed 플래그 설정
        alreadyDisposed = true;
    }

    public void ExampleMethod()
    {
        if (alreadyDisposed)
            throw new ObjectDisposedException("MyResourceHog", "Call Example Method on Disposed object");
        // 이하 생략
    }
}
```

파생 클래스의 구현 방법에 대해서도 알아보자.

```cs
public class DerivedResourceHog : MyResourceHog
{
    // 자신만의 disposed 플래그
    private bool disposed = false;

    protected override void Dispose(bool isDisposing)
    {
        // Dispose는 한 번만 수행되도록 한다.
        if (disposed)
            return;

        if (isDisposing)
        {
            // 여기서 관리 리소스를 정리한다.
        }

        // 여기서 비관리 리소스를 정리한다.

        // 베이스 클래스가 자신의 리소스를 정리할 수 있도록 해주어야 한다.
        // 베이스 클래스는 GC.SuppressFinalize()를
        // 호출해야 한다.
        base.Dispose(isDisposing);

        // 파생 클래스의 리소스가 정리되었음을 표시
        disposed = true;
    }
}
```

베이스 클래스와 파생 클래스가 각자 자신의 Dispose 여부를 나타내기 위해 고유의 플래그를 가진다.

이는 순전히 방어적으로 코드를 작성하기 위함으로 플래그를 이중으로 배치하여 베이스 클래스 혹은 파생 클래스의 일부만 정리된 경우에 혹시 발생할지도 모를 문제를 피하기 위해서다.

Dispose, finalizer는 방어적으로 작성되어야 한다.

한번 이상 호출될 수 있음을 인지하고 여러 번 호출하더라도 동일하게 동작하도록 만들어야 한다.

위 코드에서 finalizer가 없다는 것을 볼 수 있는데 이는 Dispose패턴의 구현으로 비관리 리소스를 직접 포함하지 않는 경우라면 구현하지 않는다.

항상 반드시 무조건 비관리 리소스를 포함하는 경우에만! finalizer를 구현하기를 바란다.

## 규칙

제거 혹은 정리 작업에 있어서 가장 핵심적이고 중요한 지침 중 하나는 Dispose 메서드 내에서는 리소스 정리 작업만을 수행하라는 것이다.

다른 작업은 절대로 수행해서는 안 된다.

Dispose나 finalizer에서 다른 작업을 수행하게 되면 객체의 생명주기와 관련된 심각한 문제를 일으킬 수 있다.

**객체는 생성을 시도할 때 생성돼야 하고, 가비지 수집기가 정리하려 할 때 삭제돼야 한다.**
