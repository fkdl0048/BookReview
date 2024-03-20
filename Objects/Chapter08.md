## 8장 의존성 관리하기

잘 설계된 애플리케이션은 작고 응집도 높은 객체들로 구성된다. 작고 응집도가 높은 객체란 책임의 초점이 명확하고 한 가지 일만 잘하는 객체를 의미한다. 이런 작은 객체들은 단독으로 수행할 수 있는 작업이 거의 없기 때문에 다른 객체의 도움이 필요하다. 이런 과정에서 협력을 낳는다.

객체지향 세계에서 협력은 필수적이지만, 설계를 곤경에 빠뜨릴 수 있다. 협력은 객체가 다른 객체에 대해 알아야 함을 강조한다. 즉, **다른 객체와 협력하기 위해서는 그런 객체가 존재한다는 사실을 알아야 하며 이런 지식들이 객체 사이의 의존성을 만든다.**

협력을 위해서는 의존성이 필요하지만 과도한 의존성은 애플리케이션을 수정하기 어렵게 만든다. (버그 가능성도 높아짐) 객체지향 설계의 핵심은 협력을 위해 필요한 의존성은 유지하면서도 변경을 방해하는 의존성은 제거하는데 있다.

*쉽게 스파게티 코드가 가장 협력을 많이 하는 코드가 아닐까? 실제 개개인이 자율성을 가지고 하는 작업이 효율성이 제일 잘 나오는 것처럼 코드도 관리자 주도형과 같이 하나의 컨트롤러, 매니저가 모든 일을 담당하여 처리하면 효율성과 작업의 퀄리티가 떨어질 수 있다. 이런 부분에서는 객체지향이 일상세계와 많이 닮아있다는 생각이 든다.*

### 의존성 이해하기

#### 변경과 의존성

어떤 객체가 협력하기 위해 다른 객체를 필요로 할 때 두 객체 사이에 의존성이 생기게 된다. 의존성은 실행 시점과 구현 시점에 서로 다른 의미를 가진다.

- 실행 시점: 의존하는 객체가 정상적으로 동작하기 위해서는 실행 시에 의존 대상 객체가 반드시 존재해야 한다.
- 구현 시점: 의존 대상 객체가 변경될 경우 의존하는 객체도 함께 변경된다.

```csharp
public class PeriodCondition : DiscountCondition
{
    private DayOfWeek dayOfWeek;
    private LocalTime startTime;
    private LocalTime endTime;

    public bool IsSatisfiedBy(Screening screening)
    {
        return screening.GetStartTime().GetDayOfWeek().Equals(dayOfWeek) &&
            startTime.CompareTo(screening.GetStartTime().ToLoaclTime()) <= 0 &&
            endTime.CompareTo(screening.GetStartTime().ToLoaclTime()) >= 0;
    }
}
```

`PeriodCondition`클래스의 `IsSatisfiedBy`메서드는 `Screening`인스턴스에게 `GetStartTime`메시지를 전송한다.

정리하면 실행 시점에 `PeriodCondition`의 인스턴스가 정상적으로 동작하기 위해서는 `Screening`의 인스턴스가 존재해야 한다. 