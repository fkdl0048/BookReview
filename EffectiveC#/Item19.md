# 아이템 19: 런타임에 타입을 확인하여 최적의 알고리즘을 사용하라

제네릭 타입의 경우 타입 **매개변수에 새로운 타입을 지정하여 손쉽게 재사용**할 수 있다.

타입 매개변수에 새로운 타입을 지정한다는 것은 유사한 기능을 가진 새로운 타입을 생성한다는 것을 의미한다.

제네릭을 활용하면 코드를 덜 작성해도 되기 때문에 유용하다.

하지만 **타입이나 메서드를 제네릭화하면 구체적인 타입이 주는 장점을 잃고 타입의 세부적인 특성까지 고려하여 최적화한 알고리즘을 사용할 수 없게 된다.**

`C#`은 이헌 부분까지 고려하여 설계되었다.

만약 어떤 알고리즘이 특정 타입에 대해 더 효율적으로 동작한다고 생각된다면 그냥 그 타입을 이용하도록 코드를 작성해야 한다.

이를 위해 제약 조건을 설정하는 것이 항상 효과적인 방법은 아니다.

**제네릭의 인스턴스화는 런타임을 고려하지 않으며 컴파일타임의 타입만을 고려한다.**

특정 타입의 시퀀스를 역순으로 순회하기 위해서 다음과 같이 클래스를 만들었다고 가정해보자.

```cs
public sealed class ReverseEnumerable<T> : IEnumerable<T>
{
    private class ReverseEnumerator : IEnumerator<T>
    {
        int currentIndex;
        IList<T> collection;

        public ReverseEnumerator(IList<T> srcCollection)
        {
            collection = srcCollection;
            currentIndex = collection.Count;
        }

        // IEnumerator<T> 멤버
        public T Current => collection[currentIndex];

        // IDisposable 멤버
        public void Dispose()
        {
            // 세부 구현 내용은 생략했으나 반드시 구현해야 한다.
            // 왜냐하면 IEnumerator<T>는 IDisposable을 상속받기 때문이다.
            // 이 클래스는 sealed로 선언되었으므로
            // protected Dispose() 메서드는 필요 없다.
        }

        // IEnumerator 멤버
        object System.Collections.IEnumerator.Current => Current;
        public bool MoveNext() => --currentIndex >= 0;
        public void Reset() => currentIndex = collection.Count;
    }

    IEnumerable<T> sourceSequence;
    IList<T> originalSequence;

    public ReverseEnumerable(IEnumerable<T> srcSequence)
    {
        sourceSequence = srcSequence;
    }

    // IEnumerable<T> 멤버
    public IEnumerator<T> GetEnumerator()
    {
        // 역순으로 순회하기 위해서
        // 원래 시퀀스를 복사한다.
        if (originalSequnce == null)
        {
            originalSequence = new List<T>();
            foreach (T item in sourceSequence)
            {
                originalSequence.Add(item);
            }
        }

        return new ReverseEnumerator(originalSequence);
    }

    // IEnumerable 멤버
    System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() => this.GetEnumerator();
}
```

이 코드를 살펴보면 타입 매개변수에 대한 가정이 거의 없다.

ReversEnumerable의 생성자에서 입력 매개변수가 **IEnumerable<T>를 지원한다고 가정**했을 뿐이다.

IEnumerable<T>는 개별 요소에 대한 랜덤 액세스를 지원하지 않는다.

따라서 개별 요소들을 역순으로 순회하는 유일한 방법으로 생각되는 내용이 ReverseEnumerable<T>.GetEnumerator() 를 호출한 시점에 생성자의 매개변수로 전달한 시퀀스를 이용하여 처음부터 끝까지 순회하면서 해당 요소의 복제본을 생성한다.

*만약 여기서 전달받은 IEnumerable<T>의 GetEnumerator을 this.GetEnumerator()로 설정했다면 역순이 아닌 정방향 순회가 될 것*

이 코드는 랜덤 액세스를 지원하지 않는 컬렉션에 대해서 개별 요소를 역순으로 순회하기 위한 유일한 방법이다.

하지만 대부분의 컬렉션은 랜덤 액세스를 지원하기 때문에 이와 같은 코드는 상당히 비효율적이다.

- 랜덤 액세스: 랜덤 액세스는 데이터를 저장하는 블록을 한 번에 여러 개 액세스 하는 것이 아니라 한 번에 하나의 블록만을 액세스 하는 싱글 블록 I/O 방식이다.

따라서 생성자로 전달한 인자가 IList<T>를 지원한다면 이처럼 복제본을 만들 이유가 없다.

IEnumerable<T>를 구현하고 있는 대부분의 타입들이 IList<T> 또한 구현한다는 사실에 착안하여 코드를 좀 더 효율적으로 개선해보자.

```cs
public ReverseEnumerable(IEnumerable<T> srcSequence)
{
    // 만약 sequence가 IList<T>를 구현하지 않았다면
    // originalSequence는 null이 되지만
    // 문제되지 않는다.
    sourceSequence = srcSequence;
    originalSequence = srcSequence as IList<T>;
}
```

IList<T> 타입의 매개변수를 취하는 생성자를 추가하면 되지 않을까?

이렇게 하면 매개변수가 IList<T> 타입인 것을 컴파일타임에 알 수 있으므로 구현이 더 용이할 것 같다.

하지만 이렇게 코드를 변경하게 된다면 몇몇 경우는 제대로 동작하지 않을 수 있다.

예를 들어 어떤 객체는 런타임 시에는 IList<T> 타입의 객체라 하더라도 컴파일타임에는 IEnumerable<T> 타입으로 간주될 수 있다.

이러한 경우를 대비하려면 IList<T>타입의 매개변수를 취하는 생성자 오버로드 메서드 외에도 런타임 타입을 확인하도록 코드를 작성해야 한다.

```cs
public ReverseEnumerable(IEnumerable<T> srcSequence)
{
    sourceSequence = srcSequence;
    // 만약 Sequence가 IList<T>를 구현하지 않았다면
    // originalSequence는 null이 되지만
    // 문제되지 않는다.
    originalSequence = srcSequence as IList<T>;
}

public ReverseEnumerable(IList<T> srcSequence)
{
    sourceSequence = srcSequence;
    originalSequence = srcSequence;
}
```

이제 List<T>를 사용하면 IEnumerable<T>만을 사용했을 때보다 더 효율적으로 동작하도록 코드를 개선할 수 있다.

사용자에게 더 많은 기능을 제공하는 것은 아니지만 더 효율적으로 동작하도록 성능을 개선할 수 있다.

이와 같이 코드를 변경하면 대부분의 경우 성능이 개선되지만, IList<T>를 구현하지 않고 ICollection<T>만을 구현한 컬렉션에 대해서는 여전히 비효율적으로 동작한다.

```cs
public IEnumerator<T> GetEnumerator()
{
    // 역순으로 순회하기 위해서
    // 원래 시퀀스를 복사한다.
    if (originalSequnce == null)
    {
        originalSequence = new List<T>();
        foreach (T item in sourceSequence)
        {
            originalSequence.Add(item);
        }
    }

    return new ReverseEnumerator(originalSequence);
}
```

입력 시퀀스가 ICollection<T>만을 구현한 경우 입력 시퀀스에 대한 복제본을 생성해야 하므로 매우 느리게 동작할 수밖에 없다.

다음은 ICollection<T>가 제공하는 Count 속성을 활용하여 저장소 공간을 미리 초기화하도록 코드를 조금 개선한 예다.

```cs
public IEnumerator<T> GetEnumerator()
{
    // string은 매우 특별한 경우다.
    if (sourceSequence is string)
    {
        // 컴파일타입에는 T는 char가 아닐 것이므로
        // 캐스트에 주의해야 한다.
        return new ReverseEnumerator(sourceSequence as string) as IEnumerator<T>;
    }

    // 역순으로 순회하기 위해서
    // 원래 시퀀스를 복사한다.
    if (originalSequnce == null)
    {
        if (sourceSequence is ICollection<T>)
        {
            ICollection<T> source = sourceSequence as ICollection<T>;
            originalSequence = new List<T>(source.Count);
        }
        else
        {
            originalSequence = new List<T>();
        }

        foreach (T item in sourceSequence)
        {
            originalSequence.Add(item);
        }
    }

    return new ReverseEnumerator(originalSequence);
}
```

이 코드는 입력 시퀀스로부터 리스트를 생성하는 List<T>의 생성자와 매우 유사하다.

    List<T>(IEnumerable<T> inputSequence);

실제 string의 정의를 살펴보면 IEnumerable<char>를 구현하고 있음을 알 수 있다.

    public sealed partial class String : System.Collections.Generic.IEnumerable<char>

ReverseEnumerable<T> 내에서 수행되는 매개변수에 대한 테스트가 모두 런타임에 이뤄진다는 것을 유념해야 한다.

string은 마치 IList<char>를 구현한 것처럼 랜덤액세스가 가능하지만 실제로 IList<char>를 구현한 것은 아니다.

string이 제공하는 고유의 메서드를 사용하려면 제네릭 클래스 내에 string에 특화된 코드를 작성해야만 한다.

다음은 ReverseEnumerable<T>의 중첩 클래스로 작성한 **ReversStringEnumerator** 클래스이다.

```cs
private sealed class ReverseStringEnumerator : IEnumerator<char>
{
    private string sourceSequence;
    private int currentIndex;

    public ReverseStringEnumerator(string srcString)
    {
        sourceSequence = srcString;
        currentIndex = sourceString.Length;
    }

    // IEnumerator<char> 멤버
    public char Current => sourceString[currentIndex];

    // IDisposable 멤버
    public void Dispose()
    {
        // 세부 구현 내용은 생략했으나 반드시 구현해야 한다.
        // 왜냐하면 IEnumerator<T>는 IDisposable을 상속받기 때문이다.
        // 이 클래스는 sealed로 선언되었으므로
        // protected Dispose() 메서드는 필요 없다.
    }

    // IEnumerator 멤버
    object System.Collections.IEnumerator.Current => Current;
    public bool MoveNext() => --currentIndex >= 0;
    public void Reset() => currentIndex = sourceString.Length;
}
```

이 코드를 제대로 활용하려면 ReverseEnumerable<T>.GetEnumerator()에서 타입 매개변수가 string인지 확인한 후 ReverseStringEnumerator를 생성해야 한다.

```cs
public IEnumerator<T> GetEnumerator()
{
    // string은 매우 특별한 경우다.
    if (sourceSequence is string)
    {
        // 컴파일타입에는 T는 char가 아닐 것이므로
        // 캐스트에 주의해야 한다.
        return new ReverseStringEnumerator(sourceSequence as string) as IEnumerator<T>;
    }

    // 역순으로 순회하기 위해서
    // 원래 시퀀스를 복사한다.
    if (originalSequnce == null)
    {
        if (sourceSequence is ICollection<T>)
        {
            ICollection<T> source = sourceSequence as ICollection<T>;
            originalSequence = new List<T>(source.Count);
        }
        else
        {
            originalSequence = new List<T>();
        }

        foreach (T item in sourceSequence)
        {
            originalSequence.Add(item);
        }
    }

    return new ReverseEnumerator(originalSequence);
}
```

이 코드의 목표는 제네릭 클래스 내에 타입별로 구현해야 하는 부분을 잘 숨겨두는 것이다.

string의 경우 완전히 다른 구현 방식이 필요했기 때문에 추가 작업이 조금 많아지긴 했다.

*제네릭을 정의할 때 우리가 알고 있는 것을 컴파일러가 빠짐없이 모두 이해하고 있으리라고 가정해서는 안 된다.*

간단한 에를 통해서 타입에 대한 조건을 거의 사용하지 않으면서도 타입 매개변수로 지정될 가능성이 있는 타입들의 고유한 특성을 고려하고 특화된 기능들을 최대한 활용하여 제네릭 타입을 만드는 방법을 알아봤다.