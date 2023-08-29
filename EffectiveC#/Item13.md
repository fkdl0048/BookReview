# 아이템 13: 정적 클래스 멤버를 올바르게 초기화하라

정적 멤버 변수를 포함하는 타입이 있다면 인스턴스를 생성하기 전에 반드시 정적 멤버 변수를 초기화해야 한다.

이를 위해 `C#`에서는 정적 **멤버 초기화 구문**과 **정적 생성자**라는 두 가지 기능을 제공한다.

## 정적 생성자

정적 생성자는 타입 내에 정의된 모든 메서드, 변수, 속성에 **최초로 접근하기 전에 자동으로 호출되는 특이한 메서드**다.

이 메서드를 활용하면 정적 변수를 초기화하거나, 싱글톤 패턴을 적용하거나, 혹은 여타의 작업을 효과적으로 수행할 수 있다.

정적 변수를 초기화하기 위해서 인스턴스 생성자나 전용의 private 메서드 혹은 다른 관용구를 사용해서는 안 된다.

정적 필드를 초기화하는 과정이 매우 복잡하거나 혹은 자원을 소비하는 경우라면 Lazy<T>를 사용하여 해당 필드에 최초로 접근하는 시점까지 초기화 작업을 미룰 수 있다.

## 정적 멤버 초기화 구문

인스턴스 멤버 초기화와 마찬가지로 정적 멤버를 간단히 초기화하는 경우라면 정적 생성자를 사용하기보다 멤버 초기화 구문을 사용하는 것이 좋다.

하지만 초기화 과정이 복잡하다면 정적 생성자를 사용하는 것도 나쁘지 않다.

C#에서는 정적 생성자를 사용하는 대표적인 사례로 싱글톤 패턴을 들 수 있다.

```cs
public class MySingleton
{
    private static readonly MySingleton _instance = new MySingleton(); // <- 정적 멤버 초기화 구문

    private MySingleton()
    {
    }

    public static MySingleton Instance
    {
        get
        {
            return _instance;
        }
    }
}
```

만약 간단하지 않고 초기화 과정이 더 복잡한 경우라면 다음과 같이 정적 생성자를 사용하는 것도 좋은 방법이다.

```cs
public class MySingleton
{
    private static readonly MySingleton _instance;

    static MySingleton() // <- 정적 생성자
    {
        _instance = new MySingleton();
    }

    private MySingleton()
    {
    }

    public static MySingleton Instance
    {
        get
        {
            return _instance;
        }
    }
}
```

인스턴스 멤버 초기화 구문과 마찬가지로 정적 멤버 초기화 구문 또한 정적 생성자가 호출되기 이전에 실행되며, 베이스 클래스의 정적 생성자보다도 먼저 호출된다.

## 멤버 초기화 구문 대시에 정적 생성자를 사용해야 하는 경우

멤버 초기화 구문 대시에 정적 생성자를 사용해야 하는 경우가 종종 있는데 바로 예외가 발생할 가능성이 있는 경우다.

멤버 초기화 구문의 경우 예외를 잡아낼 방법이 없기 때문에 정적 생성자를 이용하면 다음과 같이 코드를 작성할 수 있다.

```cs
public class MySingleton
{
    private static readonly MySingleton _instance;

    static MySingleton()
    {
        try
        {
            _instance = new MySingleton();
        }
        catch (Exception ex)
        {
            // 예외 처리
            // 복구 처리
        }
    }

    private MySingleton()
    {
    }

    public static MySingleton Instance
    {
        get
        {
            return _instance;
        }
    }
}
```

정적 멤버 초기화 구문과 정적 생성자는 클래스의 정적 멤버를 초기화하는 깔끔하고 명확한 방법을 제공한다.

이를 활용하면 코드를 읽기 쉽고 올바르게 작성할 수 있다.

이 기능은 다른 언어에서 정적 멤버를 초기화할 때 겪는 어려움을 언어 차원에서 해결해주기 위해서 추가되었다.

*그럼에도 정적에 대한 사용은 최소화하는 것이 좋다.*
