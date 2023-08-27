## 8. 기능 이동

지금까지는 프로그램 요소를 생성 혹은 제거하거나 이름을 변경하는 리팩터링을 다뤘다.

여기에 더해 요소를 다른 컨텍스트로 옮기는 일 역시 리팩터링의 중요한 축이다.

### 8.1 함수 옮기기(Move Function)

```csharp
class Account
{
    public GetDraftCharge() {...}
}
```

```csharp
class AccountType
{
    public GetDraftCharge() {...}
}
```

#### 배경

모듈성이란 프로그램의 어딘가를 수정하려 할 때 해당 기능과 깊이 관련된 작은 일부만 이해해도 가능하게 해주는 능력이다.

모듈성을 높이려면 서로 연관된 요소들을 함께 묶고, 요소 사이의 연결 관계를 쉽게 찾고 이해할 수 있도록 해야 한다.

하지만 프로그램을 얼마나 잘 이해했느냐애 따라 구체적인 방법이 달라질 수 있다.

보통은 이해도가 높아질수록 소프트웨어 요소들을 더 잘 묶는 새로운 방법을 깨우치게 된다.

그래서 높아진 이해를 반여하려면 요소들을 이리저리 옮겨야 할 수 있다.

모든 함수는 컨텍스트 안에 존재한다.

객체 지향 프로그래밍의 **핵심 모듈화 컨텍스트는 클래스**다.

어떤 함수가 자신이 속한 모듈 A의 요소 보다 B모듈의 요소를 더 많이 참조한다면 모듈 B로 옮겨야 마땅하다.

*이렇게 되면 캡슐화가 좋아져서 소프트웨어의 나머지 부분은 모듈 B의 세부사항에 덜 의존하게 된다.*

#### 절차

- 선택한 함수가 현재 컨텍스트에서 사용 중인 모든 프로그램 요소를 살펴본다. 이 요소들 중에도 함께 옮겨야 할 게 있는지 고민해본다.
- 선택함 함수가 다형 메서드인지 확인한다.
- 선택한 함수를 타깃 컨텍스트로 복사한다. 타깃 함수가 새로운 터전에 잘 자리 잠도록 다듬는다.
- 정적 분석을 수행한다.
- 소스 컨텍스트에서 타깃 함수를 참조할 방법을 찾아 반영한다.
- 소스 함수를 타깃 함수의 위임 함수가 되도록 수정한다.
- 테스트한다.
- 소스 함수를 인라인할지 고민해본다.

### 8.2 필드 옮기기(Move Field)

```csharp
class Customer
{
    private Plan plan;
    private string discountRate;

    public Plan GetPlan() { return plan; }
    public string GetDiscountRate() { return discountRate; }
}
```

```csharp
class Customer
{
    private Plan plan;

    public Plan GetPlan() { return plan; }
    public string GetDiscountRate() { return plan.GetDiscountRate(); }
}
```

#### 배경

프로그램의 상당 부분이 동작을 구현하는 코드로 이뤄지지만 프로그램의 진짜 힘은 데이터 구조에서 나온다.

주어진 문제를 적합한 데이터 구조를 활용하면 동작 코드는 자연스럽게 단순하고 직관적으로 짜여진다.

반면 데이터 구조를 잘못 선택하면 아귀가 맞지 않는 데이터를 다루기 위한 코드로 범벅이 된다.

이해하기 어려운 코드가 만들어지는 데서 끝나지 않고, 데이터 구조 자체도 그 프로그램이 어떤 일을 하는지 파악하기 어렵게 만든다.

그래서 데이터 구조가 매우 중요하지만, 훌룡한 프로그램이 갖춰야 할 다른 요인들과 마찬가지로, 제대로 하기가 어렵다.

현재 데이터 구조가 적절치 않다고 판단되면 바로 수정해야 한다. (이후에는 늦다.)

고치지 않고 데이터 구조에 남겨진 흠들은 우리 머릿속을 혼란스럽게 하고 훗날 작성하게 될 코드를 더욱 복잡하게 만든다.

`필드 옮기기`리팩터링은 대체로 더 큰 변경의 일환으로 수행된다.

예컨대 필드 하나를 잘 옮기면, 그 필드를 사용하던 많은 코드가 원래 위치에서 사용하는 게 더 수월할 수 있다.

그렇다면 리팩타링을 마저 호출 진행하여 호출 코드들까지 모두 변경한다.

#### 절차

- 소스 필드가 캡슐화되어 있지 않다면 캡슐화한다.
- 테스트한다.
- 타깃 객체에 필드를 생성한다.
- 정적 검사를 수행한다.
- 소스 객체에서 타깃 객체를 참조할 수 있는지 확인한다.
- 접근자들이 타깃 필드를 사용하도록 수정한다.
- 테스트한다.
- 소스 필드를 제거한다.
- 테스트한다.

#### 예시

```csharp
public class Customer
{
    private string name;
    private int discountRate;
    private CustomerContract contract;

    public Customer(string name, int discountRate)
    {
        this.name = name;
        this.discountRate = discountRate;
        this.contract = new CustomerContract(DateTime.Now);
    }

    public int GetDiscountRate()
    {
        return discountRate;
    }

    public void BecomePreferred()
    {
        ...
    }

    public void ApplyDiscount(int discountRate)
    {
        ...
    }
}

public class CustomerContract
{
    private DateTime startDate;

    public CustomerContract(DateTime startDate)
    {
        this.startDate = startDate;
    }

    public DateTime GetStartDate()
    {
        return startDate;
    }
}
```

여기서 할인율을 뜻하는 `discountRate` 필드를 `Customer`에서 `CustomerContract`로 옮기고 싶다.

```csharp
public class Customer
{
    private string name;
    private CustomerContract contract;

    public Customer(string name, int discountRate)
    {
        this.name = name;
        this.contract = new CustomerContract(DateTime.Now, discountRate);
    }

    public int GetDiscountRate()
    {
        return contract.GetDiscountRate();
    }

    public void BecomePreferred()
    {
        ...
    }

    public void ApplyDiscount(int discountRate)
    {
        ...
    }
}

public class CustomerContract
{
    private DateTime startDate;
    private int discountRate;

    public CustomerContract(DateTime startDate, int discountRate)
    {
        this.startDate = startDate;
        this.discountRate = discountRate;
    }

    public DateTime GetStartDate()
    {
        return startDate;
    }

    public int GetDiscountRate()
    {
        return discountRate;
    }
}
```

### 8.3 문장을 함수로 옮기기(Move Statements into Function)

```csharp
