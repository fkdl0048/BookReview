# 제네릭 활용

*이 내용은 3장의 제네릭 활용부분을 정리한 내용이다.*

일부 글을 좀 읽어보면 제네릭이 컬렉션과 활용될 때만 유용한 것처럼 이야기 하곤 하지만 제네릭은 컬렉션 외에도 인터페이스, 이벤트 핸들러, 공통 알고리즘 구현 등 매우 다양한 분야에서 유용하게 활용될 수 있다.

커뮤니티를 중심으로 C#의 제네릭과 C++의 템플릿을 비교하는 토론이 이뤄지곤 하는데, 어느 쪽이 더 우수하냐는 논쟁이 가장 많다.

사실 일부 관용구에선 C++ 템플릿을 이용해서 표현하는 것이 자연스럽지만, 또 다른 구조에서는 C#의 제네릭을 이용하는 편이 더 자연스럽다.

이런식의 논쟁은 이 둘의 차이점을 이해하는데 더 방해만 될 뿐이다.

```cs
public class MyType<T>
{
    public void DoSomething(T value)
    {
        // ...
    }
}
```

```cpp
template <typename T>

class MyType
{
public:
    void DoSomething(T value)
    {
        // ...
    }
};
```

## C# 제네릭

마이크로소프트의 개발팀은 제네릭 추가를 위해 C# 컴파일러는 물론이고, JIT 컴파일러, CLR등 모두를 수정했다.

*여기서 잠깐.. 자주 등장하는 JIT 컴파일러, CLR에 대해 어렴풋이 알고 있다면 확실하게 잡고 넘어가자.*

### JIT 컴파일러

JIT 컴파일러는 Just In Time 컴파일러의 약자로, 프로그램을 실행하는 시점에 기계어로 번역하는 컴파일러이다.

특징으로는 다음과 같다.

- 정적 컴파일러만큼 빠르다
- 인터프러터 언어의 빠른 응답속도를 추구한다
- 바이트코드 컴파일러가 시간이 많이 소요되는 최적화를 미리 해준다

유니티의 관점에서 설명한다면 Mono 빌드가 JIT 방식이다.

필요할 때 컴파일하는 방식으로 유니티 에디터자체가 JIT 방식으로 동작한다.

JIT방식과 상반되는 방식이 AOT(Ahead Of Time) 방식(컴파일러)이다.

AOT방식은 빌드 시점에 중간 언어를 네이티브 코드로 번역한다.

IL2CPP가 AOT방식으로 동작한다.

모든 코드를 컴파일해두기 때문에 성능은 JIT보다 좋지만 빌드가 오래 걸린다.

### CLR

CLR은 Common Language Runtime의 약자로, .NET Framework의 핵심 구성 요소이다.

다음과 같은 기능을 한다.

- C#이나 기타 언어들을 동작시키는 환경
- 메모리 관리
- 프로그램의 오류가 발생하였때 이를 처리하도록 도와주는 기능
- 언어간의 상속 지원
- COM과의 상호작용 운영성 지원

CLR은 하부구조로 OS를 가지며 OS위에서 닷넷 Application을 실행시킨다.

JVM과 같은 VM으로서 동작

JIT도 CLR의 일부이다.

---

다시 돌아와서 그렇다면 C#은 제네릭을 추가하기 위해서 거의 전부 갈아엎은 것이다.

C# 컴파일ㄹ러는 제네릭 타입으로 작성한 코드를 적절한 MSIL(Microsoft Intermediate Language)로 생성되기 위해 수정돼야 했다.

여기서 MSIL은 .NET Framework의 중간 언어이다. (IL중간언어에 자기들 MS붙인 것..)

이를 위해 사실 CLR의 동작 원리를 이해하는 것이 좋은데, 간단하게 설명하면 다음과 같다.

닷넷 특성상 다양한 플랫폼에 최적화 될 수 있는 코드를 만들어 내기 위해서 JAVA에서 하는 JVM과 같은 역할을 하는 CLR이 존재한다.

먼저 사용자가 소스 코드를 작성하면(C#으로) C# 컴파일러가 이를 MSIL로 변환한다.

그리고 CLR은 이 MSIL 읽어서 하드웨어가 이해할 수 있는 Native Code로 컴파일한다.(JIT)

앞서 말한 다양한 플랫폼에 최적화될 순 있지만, C나 C++에 비해 속도가 느리다.(컴파일 비용)

따라서 제네릭을 넣기 위해서 JIT 컴파일러는 닫힌 제네릭 타입을 생성하기 위해서 제네릭 타입에 대한 정의부와 타입 매개변수를 결합할 수 있도록 수정돼야 했다.

타입을 제네릭으로 정의하게 되면 장점도 있고 그에 따른 비용도 발생한다.

일반적으로 코드를 제네릭으로 작성하면 코드의 크기가 작아지지만 항사 그런 것은 아니다. (간혹 더 커지는 경우가 있다.)

**코드의 크기가 얼마나 작아질지 혹은 커질지는 사실 타입 매개변수를 어떻게 지정하느냐 그리고 얼마나 많은 수의 제네릭 타입을 생성하느냐에 달려 있다.**

제네릭 타입의 정의는 MSIL로 완벽하게 표현된다.

따라서 제네릭 타입 내에 포함된 코드는 타입 매개변수로 어떤 타입이 지정되더라도 유효한 코드여야 한다.

타입을 제네릭의 형태로 정의 하는 것을 **제네릭 타입 정의**라고 하며, 타입 매개변수에 구체적인 타입을 지정한 경우를 **닫힌 제네릭 타입**이라고 한다.

여러 타입 매개변수 중 일부만 구체적인 타입을 지정한 경우 이를 **열린 제네릭 타입**이라고 한다.

IL에서 제네릭은 타입을 부분적으로 정의한 형태를 띈다.

제네릭 타입으로 인스턴스를 생성할 때 반드시 필요한 타입 매개변수에 대해서는 그 위치만 표시해둔다.

JIT 컴파일러는 닫힌 제네릭 타입으로 객체를 인스턴스화하기 위한 코드를 생성할 때 비로소 타입의 정의를 완성한다.

여러 형태의 닫힌 제네릭 타입을 미리 만들어두면 코드의 크기가 너무 커지고 타입 정의 부분적으로나마 완성해두지 않으면 성능이 너무 느려질 것이기 때문이다.

이러한 방식은 시간과 공간을 고려한 절충안이라 한다.

뭔가 로우레벨 코드를 보다보면 생각보다 우아하게 짜지 못한 코드들이 종종 존재한다.(과연 억지스러운가, 우아하지 못한가?)

*뭔가 딱 맞아 떨어지지 않아서 그렇게 생각하는 것 같다.*

이는 트레이드 오프를 이해하지 못했을 때의 내가 생각했던 불편한 코드에 대한 이야기고, 사실 이런 베이스 위에 사람이 읽기 좋은 코드를 짜고 있는 것이 아닐까 싶다.

**모든 것은 트레이드 오프이다.**

제네릭 타입이 아니라면 클래스를 표현하는 IL과 이로부터 생성되는 머신 코드가 항상 일대일 관계를 유지한다.

제네릭을 사용하면 그 과정이 조금 다르다.

JIT 컴파일러가 제네릭 클래스를 만나게 되면 타입 매개변수로 주어진 타입을 확인하여 해당 타입에 가장 적합한 머신 코드를 생성하기 위해 노력한다.

그 예의 하나로 **제네릭 타입의 타입 매개변수로 참조 타입이 전달되는 경우에는 항상 동일한 머신 코드가 생성되며 이를 공유한다.**

*실제로 아래 코드에 나타난 개별 인스턴스들은 런타임에 동일한 머신 코드를 공유한다.*

```cs
List<string> stringList = new List<string>();
List<Stream> OpenFiles = new List<Stream>();
List<MyClassType> anotherList = new List<MyClassType>();
```

C# 컴파일러는 컴파일타임에 타입 안전성을 미리 확인하기 때문에 JIT 컴파일러는 이러한 전제를 기반으로 더 최적화된 머신 코드를 생성할 수 있다.

반대로 닫힌 제네릭 타입을 만들 때 타입 매개변수에 값 타입이 지정되면 참조 타입과 다른 규칙이 적용된다.

JIT 컴파일러는 타입 매개변수에 서로 다른 값 타입이 주어지는 경우, 서로 다른 머신 코드를 생성한다.

```cs
List<int> markers = new List<int>();
List<double> doubleList = new List<double>();
List<MyStruct> values = new List<MyStruct>();
```

흥미로운 내용이지만 왜 이런 것까지 신경 써야 하는 걸까?

## 정리

**제네릭 타입의 타입 매개변수로 참조 타입이 주어지는 경우, 그것이 어떤 타입이라 하더라도 메모리의 풋프린트에는 전혀 영향을 주지 않는다.**

JIT 컴파일된 코드가 공유되기 때문이다.

하지만 타입 매개변수로 서로 다른 값 타입이 주어지는 경우, 각자 서로 다른 머신 코드가 생성되기 때문에 이는 공유될 수 없다.

이러한 특성이 코드에 어떤 영향을 미치는지 이해하려면 각각의 과정을 좀 더 세부적으로 살펴봐야 한다.

.NET 런타임이 제네릭 정의(메서드든 클래스든)를 JIT컴파일 할 때 타입 매개변수에 값 타입이 지정되면 두 단계를 거치게 된다.

첫 번째 단계는 **닫힌 제네릭 타입을 표현하기 위한 새로운 IL클래스를 생성**하는 것이다.

극도로 단순화하자면 JIT 컴파일러의 역할은 제네릭 정의에 포함된 T를 int와 같이 값 타입으로 대체하는 것이다.

두 번째 단계는 대체가 완료된 타입을 이용하여 실제로 머신 코드를 생성하는 것이다.

이처럼 두 단계가 필요한 이유는 어셈블리가 로드되는 시점에 JIT 컴파일러가 클래스의 머신 코드를 생성하는 것이 아니라, 로드된 타입 내의 특정 메서드가 최초로 호출되는 시점에 호출된 메서드만을 JIT 컴파일하기 때문이다.

JIT 컴파일이 수행되고 나면 메서드 내의 IL 코드가 앞서 컴파일된 머신 코드로 대체된다. (당연히 이렇게 돼야 한다.)

이러한 과정은 제네릭 타입이 아닌 일반 타입의 경우에도 동일하다.

그런데 이처럼 타입별로 서로 다른 코드가 생성된다는 것을 런타임에 메모리의 풋프린트가 커진다는 사실을 의미하는 것이기도 하다.

타입 매개변수로 값 타입을 취하는 닫힌 제네릭 타입은 개별적인 IL 코드를 가진다.

타입 매개변수로 지정한 값 타입의 유형에 따라 서로 다른 머신 코드를 생성할 수 밖에 없기 때문이다.

하지만 제네릭 타입의 타입 매개변수로 값 타입을 지정하면 [박싱과 언박싱](https://fkdl0048.github.io/csharp/cshape_Boxing/)을 피할 수 있고, 결국 코드와 데이터의 크기는 줄어드는 장점이 있다.

그리고 컴파일러가 타입 매개변수에 대한 타입 안정성을 보장해주므로 런타임에 타입의 유형을 매번 확인할 필요가 없다.

따라서 코드의 크기가 줄고 성능이 개선된다.