# 아이템 3: 캐스트보다 is, as가 좋다

`C#`은 정적 타이핑을 수행하는 언어이다.

따라서 타입 불일치가 발생하더라도 컴파일러가 이를 걸러주기 때문에 런타임에 타입 검사를 자주 수행할 필요가 없다.

하지만 간혹 반드시 런타임에 타입을 확인해야 하는 경우도 있다. `C#`의 경우 `.NetFramwokr`에서 정의해둔 메서드의 원형에 따라 `object`타입의 인자를 취하도록 메서드를 정의해야 하는 경우가 있다.

> IComparer가 그 예이다.

이렇게 전달 된 매개변수는 다른 클래스나 인터페이스로 형 변환 후 사용이 된다.

`C#`에서 형 변환을 수행하는 방법에는 as연산자를 사용하는 방법과 컴파일러의 캐스트 연산자 구문을 사용하는 두가지 방법이 있다.

더 방어적인 코드를 작성하려는 경우는 우선 **`is`**연산자로 형변환이 가능한지를 확인 후에 실제 형변환을 수행하도록 코드를 작성할 수 있다.

형 변환을 수행하는 경우 캐스팅으 사용하기보다 as연산자를 사용하는 것이 좋다..!  

as연산자는 안전하기도 하지만 런타임에 더 효율적으로 동작한다.

*다만 as나 is연산자를 사용하면 사용자 정의 형변환은 수행되지 않는다.*

이런 이유로 런타임에 객체의 타입이 변환하려는 타입과 정확히 일치하는 경우에만 형변환이 성공적으로 수행된다.

형변환 과정에서 새로운 객체가 생성되는 경우는 거의 없다

*예외적으로 as연산자를 이용하여 박싱된 값 타입의 객체를 nullable 값 타입의 객체로 변환하는 경우 새로운 객체가 생성된다.*

## 예제 1번

```cs
object o = Factory.GetObject();

MyType t = o as MyType;

if (t != null)
{
    // MyType타입의 t객체 사용
}
else
{
    // 오류 보고
}
```


```cs
object o = Factory.GetObject();

try
{
    MyType t;
    t = (MyType)o; 
    // MyType타입의 t객체 사용
}
catch (InvalidCastException e)
{
    // 오류 보고
}
```

1번 코드는 작성하기 쉽고, 읽기도 편하다

try/catch문이 없기 때문에 성능까지 챙긴 모습이다.

캐스팅을 사용한 2번 코드는 null값을 확인할 필요가 없어 보이지만 null은 기본적으로 어떤 참조 타입으로도 형변환될 수 있기 때문에 반환값이 null인지를 여전히 확인해야 한다.

`as`연산자는 형변환을 수행할 수 없거나, null을 대상으로 형변환을 수행하는 경우 null을 반환한다.

이러한 이유로 캐스팅을 사용하면 예외처리 코드와 null확인 코드가 모두 필요하지만 as 연산자를 사용하면 null확인 코드만 있으면 된다.

## as연산자와 캐스팅의 차이

as나 is 연산자는 런타임에 객체의 타입을 확인하고 필요에 따라 박싱을 수행하는 것을 제외하고는 어떠한 작업도 수행하지 않는다.

임의의 객체를 다른 타입으로 형변환하려면 이 객체는 지정한 타입이거나 혹은 지정된 타입을 상속한 타입이어야 한다.

그 외의 경우는 모두 실패한다.

**반면** 캐스팅을 사용하는 경우에는 객체를 지정한 타입으로 변환하기 위해서 형변환 연산자가 개입될 수 있다.

대표적인 형변환 연산자가 바로 숫자 타입에 대한 형변환 연산자다.

*long타입을 short타입으로 캐스팅하면 일부 정보는 소실된다.*

* implicit/explicit 연산자를 사용하는 경우에는 컴파일러가 형변환 연산자를 사용할지 말지를 결정한다.

implicit 형변환 연산자: 자동 형변환을 수행. 이 연산자를 사용하면 명시적으로 형변환을 해주지 않아도 자동으로 형변환이 수행된다.  

explicit 형변환 연산자: 명시적 형변환을 수행. 이 연산자를 사용하면 형변환을 명시적으로 수행한다.

```cs
public class SecondType
{
    private MyType _myType;

    // 중략

    // 형변환 연산자
    // SecondType을 MyType으로 형변환
    public static implicit operator MyType(SecondType t)
    {
        return t._value;
    }
}
```

```cs
object o = Factory.GetObject();

// o는 SecondType이다.
// 첫 번째 버전
MyType t = o as MyType;

if (t != null)
{
    // MyType타입의 t객체 사용
}
else
{
    // 오류 보고
}

// 두 번째 버전
try
{
    MyType t;
    t = (MyType)o; // o가 MyType이 아니면 예외 발생
    // MyType타입의 t객체 사용
}
catch (InvalidCastException e)
{
    // 오류 보고
}
```

이 두가지 버전 모두 형변환에 실패한다.

앞서 캐스팅을 사용하면 사용자 정의 형변환 연산자가 수행된다고 말했기 때문에 두 번째 버전은 성공했어야 하지만 컴파일러는 런타임에 객체가 어떤 타입일지를 예측하지 못한다.

컴파일러는 단순히 컴파일타임에 객체가 어떤 타입으로 선언됐는지만 추적하기 때문에 두 번째 버전 또한 실패한다.

컴파일러는 객체 o가 어떤 타입인지 알 수 없다.

따라서 객체 o는 object타입이라고 생각하고 object타입을 MyType으로 형변환할 수 있는 연산자가 정의됐는지만 확인한다.

이러한 형변환 연산자를 정의하지 않았으므로 컴파일러는 이제 o객체가 MyType 형식인지를 확인하는 코드만 생성한다.

불행히도 런타임에 o는 SecondType형식의 객체이므로 형변환에 실패한다.

> 사전에 적절한 검사를 수행해서 예외가 가능한 한 발생하지 않도록 코드를 작성하는 것이 보편적인 프로그래밍 방식이다.

> t = (MyType)st;

이 코드는 st가 어떤 타입으로 선언되었든 항상 동일한 결과를 반환한다.

이처럼 일관성이 높기 때문에 캐스팅보다 as를 사용하는 것이 좋다..

> t = st as MyType;

## as를 사용할 수 없는 경우

```cs
object o = Factory.GetValue();
int i = o as int; // 컴파일 오류
```

int는 값 타입이고 null이 될 수 없기 때문에 as 연산자를 사용할 수 없다.

as연산자를 그대로 이용하되 nullable타입으로 형 변환을 수행한 후 그 값이 null인지를 확인하면 된다.

```cs
object o = Factory.GetValue();
var i = o as int?;
if (i != null)
{
    // i.Value를 사용
}
```

이러한 방법은 as 연산자의 왼쪽 피연산자가 값 타입이거나 nullable 값 타입일 경우 사용할 수 있다.

## foreach문에서 as를 사용하는 경우

foreach루프는 제네릭 타입이 아닌 IEnumerable 인터페이스를 이용한다. (`C#` 특성)  

그리고 실제로 이를 구현하는 과정에서 형변환을 수행한다.

```cs
public void UseCollection(IEnumerable theCollection)
{
    foreach (MyType t in theCollection)
    {
        t.DoSomething();
    }
}
```

foreach문은 캐스팅을 통해서 루프에서 사용되는 타입으로 객체를 형변환한다.

foreach문이 생성하는 코드는 다음과 유사하다.

```cs
public void UseCollection(IEnumerable theCollection)
{
    IEnumerator enumerator = theCollection.GetEnumerator();
    try
    {
        while (enumerator.MoveNext())
        {
            MyType t = (MyType)enumerator.Current;
            t.DoSomething();
        }
    }
    finally
    {
        IDisposable disposable = enumerator as IDisposable;
        if (disposable != null)
        {
            disposable.Dispose();
        }
    }
}
```

IEnumerator.Current는 System.Object 타입의 객체를 반환한다.

그리고 System.Object는 어떤 형변환 연산자도 포함하고 있지 않다.

foreach문장 (캐스팅을 사용하는)은 컬렉션 내부의 객체가 런타임이 어떤 타입인지, System.Object타입으로 형변환 가능한지와 Object타입의 객체를 루프 변수 타입으로 형 변환 가능한지만을 확인할 뿐이다.

따라서 캐스트연산보다 is너 as를 사용하는 것이 의도하지 않은 부작용이나 예상치 못한 문제를 피할 수 있는 좋은 방법이다.

