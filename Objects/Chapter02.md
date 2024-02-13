## 2장 객체지향 프로그래밍

기술적인 글을 슬 때 어려운 부분은 적당한 수준의 난이도와 복잡도를 유지하면서 이해하기 쉬운 예제를 선택하는 것이다. 예제가 너무 복잡하면 주제를 효과적으로 전달하기 어렵고, 너무 간단하면 현실성이 떨어지기에 그 균형을 맞추기가 어렵다.

이 책 역시 일반적인 기술 서적이 가지는 한계를 벗어나지 못한다는 점을 미리 밝혀 두고자 한다. 이번 장에서 다루는 내용 역시 복잡하지는 않지만 매우 크고 복잡한 시스템이라고 최면을 걸어주길 바란다.

### 영화 예매 시스템

#### 요구사항 살펴보기

- 영화 예매 시스템 요구사항 분석
  - '영화' 영화에 대한 기본 정보를 표현한다.
    - 기본적인 제목, 상영시간, 가격 정보와 같이 영화가 가지고 있는 정보를 가리킬 때 '영화'라는 단어를 사용한다.
  - '상영'은 실제로 관객들이 영화를 관람하는 사건을 표현한다.
    - 상영 일자, 시간, 순번 등을 가리키기 위해 '상영'이라는 단어를 사용한다.
    - 사용자가 실제로 예매하는 대상은 영화가 아닌 상영이다.
  - 하나의 영화는 하루 중 다양한 시간대에 걸쳐 한 번 이상 상영될 수 있다.
  - 특정한 조건을 만족하는 예매자는 할인받을 수 있다.
    - 할인 조건(discount condition)
      - 할인조건은 가격의할인 여부를 결정하며 순서 조건과 기간 조건으로 나뉜다.
        - 순서 조건은 상영 순번을 이용해 할인 여부를 결정한다.
        - 기간 조건은 영화 상영 시작 시간을 이용해 할인 여부를 결정한다.
          - 기간 조건은 요일, 시작 시간, 종료 시간의 세 부분으로 구성되며 영화 시작 시간이 해당 기간안에 포함될 경우 할인을 적용한다.
    - 할인 정책(discount policy)
      - 할인 정책은 금액 할인 정책과 비율 할인 정책이 있다.
        - 금액 할인 정책은 예매 요금에서 일정 금액을 할인해주는 방식
        - 비율 할인 정책은 예매 요금에서 일정 비율을 할인해주는 방식
    - 영화별로 하나의 할인 정책만 할당할 수 있다. 지정하지 않는 것도 가능
    - 할인 조건은 다수의 할인 조건을 함께 지정할 수 있으며, 순서 조건과 기간 조건을 섞는 것도 가능하다.

위 요구사항대로 할인을 적용하기 위해서는 할인 조건과 할인 정책을 함께 조합해서 사용한다. 먼저 사용자의 예매 정보를 통해 할인 조건 중 하나라도 만족하는지 검사한다. 할인 조건을 만족할 경우 할인 정책을 이용해 할인 요금을 계산한다. 할인 정책은 적용돼 있지만 할인 조건을 만족하지 못하는 경우나 아예 할인 정책이 적용돼 있지 않은 경우엔 할인하지 않는다.

### 객체지향 프로그래밍을 향해

#### 협력, 객체, 클래스

대부분의 사람이 객체지향 언어로 코딩할 때 가장 먼저 작성하게 되는 것은 클래스로 클래스가 결정된 이후에 속성과 메서드를 결정한다. 하지만 이는 객체지향의 본질과 거리가 있으며 객체지향은 말 그대로 객체를 지향하는 것이다.

진정한 객체지향 패러다임의 전환은 클래스가 아닌 객체에 초점을 맞출 때에만 얻을 수 있다. 이를 위해선 다음에 집중해야 한다.

- 어떤 클래스가 필요한지를 고민하기 전에 어떤 객체들이 필요한지 고민하라.
  - 클래스는 공통적인 상태와 행동을 공유하는 객체들을 추상화한 것이다. (정적모델)
  - 클래스의 윤곽을 잡기 위해서는 어떤 객체들이 어떤 상태와 행동을 가지는지를 먼저 결정해야 한다.
  - 객체를 중심에 두는 접근 방법은 설계를 단순하고 깔끔하게 만든다. (즉, 필요 이상의 구현을 하지 않음)
- 객체는 독립적인 존재가 아니라 기능을 구현하기 위해 협력하는 공동체의 일원으로 봐야 한다. (어떤 객체도 섬이 아니다.)
  - 객체를 협력하는 공동체의 일원으로 바라보는 것은 설계를 유연하고 확장 가능하게 만든다.
  - 객체지향적으로 생각하고 싶다면 객체를 고립된 존재로 바라보지 말고 협력에 참여하는 협력자로 바라봐야 한다.
  - 객체들의 모양과 윤곽이 잡히면 공통된 특성과 상태를 가진 객체들을 타임으로 분류하고 이 타입을 기반으로 클래스를 구현한다.

*1장에서 정리했지만 중요하기에 반복해서 적는다. 설계의 기초는 객체의 동적 모델을 먼저 도메인 모델에 맞게 만들고, 그에 맞는 인터페이스(메시지)를 만든 뒤 이 객체들 사이의 협력 공동체를 설계하는 것이다. 그 이후에 정적모델인 클래스로 동적 모델을 투사하여 각각 객체에 올바른 책임을 할당한 뒤 객체들의 협력에서 스스로, 자율적으로 역할을 수행하는 것이다.*

#### 도메인의 구조를 따르는 프로그램 구조

이 시점에서 **도메인(domain)**이라는 용어를 살펴보는 것이 도움이 될 것이다. 소프트웨어는 사용자가 원하는 어떤 문제를 해결하기 위해 만들어진다. 영화 예매 시스템의 목적은 영화를 좀 더 쉽고 빠르게 예매하려는 사용자의 문제를 해결하는 것이다.

이처럼 문제를 해결하기 위해 사용자가 프로그램을 사용하는 분야를 **도메인**이라고 부른다.

객체지향 패러다임이 강력한 이유는 요구사항을 분석하는 초기 단계부터 프로그램을 구현하는 마지막 단계까지 객체라는 동일한 추상화 기법을 사용할 수 있기 때문이다. 요구사항과 프로그램 객체라는 동일한 관점에서 바라볼 수 있기 때문에 도메인을 구성하는 개념들이 프로그램의 객체와 클래스로 매끄럽게 연결될 수 있다.

- [객체지향의 사실과 오해 도메인 관련 글](https://github.com/fkdl0048/BookReview/blob/main/Object-oriented_Facts_and_Misunderstandings/Chapter06.md#%EB%8F%84%EB%A9%94%EC%9D%B8-%EB%AA%A8%EB%8D%B8)

![image](https://github.com/fkdl0048/BookReview/assets/84510455/ac0b5692-34a6-4aef-ae3c-ac2592b67627)

이 그림은 영화 예매 도메인을 구성하는 개념과 관계를 표현한 것이다. 영화는 여러 번 상영될 수 있고 상영은 여러 번 예매될 수 있다는 것을 알 수 있다. 영화에는 할인 정책을 할당하지 않거나 할당하더라도 오직 하나만 할당할 수 있고, 할인 정책이 존재하는 경우에는 하나 이상의 할인 조건이 반드시 존재한다는 것을 알 수 있다.

*이러한 도메인 모델은 설계 레벨에서 바로 작성하는 것이 아닌 요구사항을 충분히 분석 후 글로 적어도 문제가 없다면 그림으로 그려보는 것도 좋다. 가장 좋은 점은 이런 다이어그램은 이해관계자들에게 설명할 때 효과적이라는 것이다. 지금 이 책을 읽고 있는 나에게 저자는 가장 효과적인 방법을 사용한 것.*

자바나 C#같은 클래스 기반의 객체지향 언어에 익숙하다면 도메인 개념을 구현하기 위해 클래스를 사용한다는 사실이 낯설지는 않을 것이다. 일반적으로 클래스의 이름은 대응되는 **도메인 개념의 이름과 동일하거나 적어도 유사하게 지어야 한다.** 클래스 사이의 관계도 최대한 도메인 개념 사이에 맺어진 관계와 유사하게 만들어서 프로그램의 구조를 이해하고 예상하기 쉽게 만들어야 한다.

#### 클래스 구현하기

도메인 개념들의 구조를 반영하는 적절한 클래스 구조를 만들었다면 이제 남은 일은 적절한 프로그래밍 언어를 이용해 이 구조를 표현하는 것이다.

*구현하기 앞서 뒤에서 설명할지 모르겠지만, 객체간의 협력의 메시지를 결정하는 것이 우선이 되어야 한다. 이를 꼭 유스케이스 형태로 표현할 필요는 없지만, 객체간의 메시지를 통한 객체의 역할과 책임을 결정하는 것이 선행되어야 한다. 아마 책에선 뒷 부분에서 나올 것이라 예상되지만 이번 장은 들어가는 말에서 언급한 것 처럼 객체지향 프로그래밍을 보여주기 위한 챕터인 듯 하다.*

`Screening` 클래스는 사용자들이 예매하는 대상인 '상영'을 구현한다. Screening은 상영할 영화(movie), 순번(sequence), 상영 시작 시간(whenScreened)을 인스턴스 변수로 포함한다. `Screening`은 상영 시작 시간을 반환하는 `GetStartTime()` 메서드, 순번의 일치 여부를 검사하는 `IsSequence()` 메서드, 기본 요금을 반환하는 `GetMovieFee()` 메서드를 포함한다.

```cs
class Screening
{
    private Movie movie;
    private int sequence;
    private DateTime whenScreened;

    public Screening(Movie movie, int sequence, DateTime whenScreened)
    {
        this.movie = movie;
        this.sequence = sequence;
        this.whenScreened = whenScreened;
    }

    public DateTime GetStartTime() => whenScreened;
    public bool IsSequence(int sequence) => this.sequence is sequence;
    public Money GetMovieFee() => movie.GetFee();
}
```

여기서 주목할 점은 인스턴스 변수의 가시성은 private이고 메서드의 가시성은 public이라는 것이다. **클래스를 구현하거나 다른 개발자에 의해 개발된 클래스를 사용할 때 가장 중요한 것은 클래스의 경계를 구분 짓는 것이다.**

클래스는 내부와 외부로 구분되며 훌륭한 클래스를 설계하기 위한 핵심은 어떤 부분을 외부에 공개하고 어떤 부분을 감출지를 결정하는 것이다. Screening에서 알 수 있는 것처럼 외부에서는 객체의 속성에 직접 접근할 수 없도록 막고 적절한 public 메서드를 통해서만 내부 상태를 변경할 수 있게 해야 한다.

**이렇게 내부와 외부를 구분하는 이유는 경계의 명확성이 객체의 자율성을 보장하기 때문이다. 그리고 더 중요한 이유로 프로그래머에게 구현의 자유를 제공하기 때문이다.**

##### 자율적인 객체

자율적인 객체를 이해하기 앞서 두 가지 중요한 사실을 정리해야 한다.

- 첫 번째 사실은 객체가 **상태(state)**와 **행동(behavior)**을 함께 가지는 복합적인 존재라는 것이다.
- 두 번째 사실은 객체가 스스로 판단하고 행동하는 **자율적인 존재**라는 것이다.

*두 가지 사실은 서로 깊이 연관돼 있다.*

- 데이터와 기능을 객체 내부로 함께 묶는 것을 **캡슐화**라고 한다.
- 외부에서의 접근을 통제할 수 있는 **접근 제어** 메커니즘도 함께 제공한다.
  - public, protected, private와 같은 **접근 수정자**를 제공한다.

캡슐화와 접근 제어는 객체를 두 부분으로 나눈다. 하나는 외부에서 접근 가능한 부분으로 이를 **퍼블릭 인터페이스(public interface)**라고 부른다. 다른 하나는 외부에서는 접근 불가능하고 오직 내부에서만 접근 가능한 부분으로 이를 **구현(implementation)**이라고 부른다. (DIP원칙의 중요성은 뒤에서 다룸)

##### 프로그래머의 자유

프로그래머의 역할을 **클래스 작성자(class creator)**와 **클라이언트 프로그래머(client programmer)**로 구분하는 것이 유용하다. 클래스 작성자는 새로운 데이터 타입을 프로그램에 추가하고, 클라이언트 프로그래머는 클래스 작성자가 추사한 데이터 타입을 사용한다.

클라이언트 프로그래머의 목표는 필요한 클래스들을 엮어서 애플리케이션을 빠르고 안정적으로 구축하는 것이다. 클래스 작성자는 필요한 부분만 공개하고 나머지는 꽁꽁 숨겨야 한다. 숨겨진 나머지 부분을 클라이언트 프로그래머가 접근하지 못함으로서 그에 대한 영향을 걱정하지 않고도 내부 구현을 마음대로 변경할 수 있다. 이를 **구현 은닉(implementation hiding)**이라고 부른다.

접근 제어 메커니즘은 프로그래밍 언어 차원에서 클래스의 내부와 외부를 명확하게 경계 지을 수 있게 하는 동시에 클래스 작성자가 내부 구현을 은닉할 수 있게 해준다. 또한 클라이언트 프로그래머가 실수로 숨겨진 부분에 접근하는 것을 막아준다.

객체의 외부와 내부를 구분하면 클라이언트 프로그래머가 알아야 할 지식의 양이 줄고(추상화) 클래스 작성자가 자유롭게 구현을 변경할 수 있는 폭이 넓어진다. 따라서 클래스를 개발할 때마다 인터페이스와 구현을 깔끔하게 분리하기 위해 노력해야 한다.

설계가 필요한 이유는 변경을 관리하기 위해서라는 것을 기억하자. 객체지향 언어에서 객체 사이의 의존성을 적절히 관리함으로써 변경에 관리함으로써 변경에 대한 파급효과를 제어할 수 있는 다양한 방법을 제공한다. 객체의 변경을 관리할 수 있는 기법 중 가장 대표적인 것이 **접근 제어**이다.

*앞서 다룬 캡슐화에 관한 내용으로 프로그래머의 역할로 바라봤다.*

#### 협력하는 객체들의 공동체

```cs
class Screening
{
    public Resevation Reserve(Customer customer, int adienceCount)
    {
        return new Resevation(customer, this, CalculateFee(audienceCount), audienceCount);
    }

    private Money CalculateFee(int audienceCount)
    {
        return movie.CalculateFee(this).times(audienceCount);
    }
}
```

- `Screening`의 `Reserve()`메서드는 영화를 예매한 후 예매 정보를 담고 있는 `Reservation`의 인스턴스를 생성해서 반환한다.
- `CalculateFee()`메서드는 요금을 계산하기 위해 다시 `Movie`의 `CalculateFee()`메서드를 호출한다.
- `Movie`의 `CalculateFee()`는 1인당 예매 요금이다. 따라서 `Screening`은 전체 예매 요금을 구하기 위해 `CalculateFee()` 메서드의 반환값에 인원 수인 audienceCount를 곱한다.

```cs
class Money
{
    public static const Money ZERO = Money.Wons(0);

    private const BigDecimal amount;

    public static Money Wons(double amount) => return new Money(BigDecimal.valueOf(amount));
    
    public Money(BigDecimal amount)
    {
        this.amount = amount;
    }

    public Money Plus(Money amount)
    {
        return new Money(this.amount.add(amount.amount));
    }

    public Money Minus(Money amount)
    {
        return new Money(this.amount.subtract(amount.amount));
    }

    public Money Times(double percent)
    {
        return new Money(this.amount.multiply(BigDecimal.valueOf(percent)));
    }

    public bool IsLessThan(Money other)
    {
        return amount.compareTo(other.amount) < 0;
    }

    public bool IsGreaterThanOrEqual(Money other)
    {
        return amount.compareTo(other.amount) >= 0;
    }
}
```

객체지향의 장점은 객체를 이용해 도메인의 의미를 풍부하게 표현할 수 있다는 것이다. 따라서 의미를 좀 더 명시적이고 분명하게 표현할 수 있다면 객체를 사용해서 개념을 구현하는 것이 좋다. 그 개념이 비록 하나의 인스턴스 변수만을 포함하더라도 개념을 명시적으로 표현하는 것은 전체적인 설계의 명확성고 유연성을 높이는 방법이다.

*비슷한 예로 인터페이스가 단일적으로 필요하더라도 선언하여 사용하는 것이 좋을 수 있다는 예가 있다.*

```cs
class Reservation
{
    private Customer customer;
    private Screening screening;
    private Money fee;
    private int audienceCount;

    public Reservation(Customer customer, Screening screening, Money fee, int audienceCount)
    {
        this.customer = customer;
        this.screening = screening;
        this.fee = fee;
        this.audienceCount = audienceCount;
    }
}
```

영화를 예매하기 위해 `Screening`, `Moview`, `Reservation` 인스턴스들은 서로의 메서드를 호출하며 상호작용한다. 이 처럼 시스템의 어떤 기능을 구현하기 위해 객체들 사이에 이뤄지는 상호작용을 **협력(Collaboration)**이라고 부른다.

객체지향 프로그램을 작성할 때는 먼저 협력의 관점에서 어떤 객체가 필요한지를 결정하고, 객체들의 공통 상태와 행위를 구현하기 위해 클래스를 작성한다.

#### 협력에 관한 짧은 이야기

앞에서 설명한 것처럼 객체의 내부 상태는 외부에서 접근하지 못하도록 감춰야 한다. 대신 외부에 공개 하는 퍼블릭 인터페이스를 통해 내부 상태에 접근할 수 있도록 허용한다. 객체는 다른 객체의 인터페이스에 공개된 행동을 수행하도록 **요청(request)**할 수 있다. 요청을 받는 객체는 자율적인 방법에 따라 요청을 처리한 후 **응답(response)**한다.

- [협력에 관한 객체지향의 사실과 오해 관련 글](https://github.com/fkdl0048/BookReview/blob/main/Object-oriented_Facts_and_Misunderstandings/Chapter04.md#%ED%98%91%EB%A0%A5)

객체가 다른 객체와 상호작용할 수 있는 유일한 방법은 **메시지를 전송(send a message)**하는 것뿐이다. 다른 객체에게 요청이 도착할 때 해당 객체가 **메시지를 수신(receive a message)**했다고 이야기한다. 메세지를 수신한 객체는 스스로의 결정에 따라 자율적으로 메세지를 처리할 방법을 결정한다. 이처럼 수신된 메시지를 처리하기 위한 자신만의 방법을 **메서드(method)**라고 부른다.

**메세지와 메서드를 구분하는 것은 매우 중요하다.** 객체지향 패러다임이 유연하고, 확장 가능하며, 재사용 가능한 설계를 낳는다는 명성을 얻게 된 배경에는 메시지와 메서드를 명확하게 구분한 것도 단단히 한 몫한다. 뒤에서 나오지만 메세지와 메서드의 구분에서 **다형성(polymorphism)**의 개념이 출발한다.

*지금까지는 Screening이 Movie의 CalculateMovie 메서드를 호출한다고 말했지만 사실은 Screening이 Movie에게 CalculateMovieFee 메시지를 전송한다라고 말하는 것이 더 적절한 표현이다. 사실 Screening은 Movie안에 CalculateMovieFee 메서드가 존재하고 있는지조차 알지 못한다.단지 Movie가 CalculateMovieFee메시지에 응답할 수 있다고 믿고 메시지를 전송할 뿐이다.*

### 할인 요금 구하기

#### 할인 요금 계산을 위한 협력 시작하기

계속해서 예매 요금을 계산하는 협력을 알아보면, Movie는 제목, 상영시간, 기본요금, 할인 정책을 속성으로 가진다. 이 속성들의 값은 생성자를 통해 전달받는다.

```cs
class Movie
{
    private string title;
    private Duration runningTime;
    private Money fee;
    private DiscountPolicy discountPolicy;

    public Movie(string title, Duration runningTime, Money fee, DiscountPolicy discountPolicy)
    {
        this.title = title;
        this.runningTime = runningTime;
        this.fee = fee;
        this.discountPolicy = discountPolicy;
    }

    public Money GetFee() => fee;
    public Money CalculateMovieFee(Screening screening) => fee.minus(discountPolicy.CalculateDiscountAmount(screening));
}
```

`CalculateMovieFee()`메서드는 `discountPolicy`에 `CalculateDiscountAmount`메시지를 전송해 할인 요금을 반환받는다. Movie는 기본요금인 fee에서 반환된 할인 요금을 차감한다.

이 코드에서 중요한 점은 어떠한 할인 정책을 사용할 것인지에 대한 코드가 어디에도 존재하지 않는다. 도메인을 설명할 때 언급했던 것처럼 영화 예매 시스템에는 두 가지 종류의 할인 정책이 존재한다. 하나는 일정한 금액을 할인해주는 금액 할인 정책이고 다른 하나는 일정한 비율에 따라 할인해주는 비율 할인 정책이다.

따라서 예매 요금을 계산하기 위해서는 현재 영화에 적용돼 있는 할인 정책의 종류를 판단할 수 있어야 한다. 하지만 코드 어디에도 할인 정책을 판단하는 코드는 존재하지 않는다. 단지 `DiscountPolicy`에게 메시지를 전송할 뿐이다.

*여기서 바로 찾아낸다면 객체지향에 대한 기초는 있는 것이라 봐도 무방하지만 한번에 이해가 되지 않는다면 익숙하지 않은 것으로 봐야 한다.*

이 코드에는 **상속**과 **다형성**이라는 개념이 숨어있고 그 기반은 **추상화**라는 원리가 숨겨져 있다.

#### 할인 정책과 할인 조건

할인 정책은 금액 할인 정책과 비율 할인 정책으로 구분된다. 두 가지 할인 정책을 각각 AmountDiscountPolicy와 PercentDiscountPolicy라는 클래스로 구현한다. 두 클래스는 대부분의 코드가 유사하고 할인 요금을 계산하는 방식만 족므 다르다. 따라서 두 클래스 사이의 중복 코드를 제거하기 위해 공통으로 코드를 보관할 장소가 필요하다.

DiscountPolicy라는 추상 클래스를 만들어 두 클래스의 공통 코드를 이 클래스로 옮긴다.

```cs
abstract class DiscountPolicy
{
    private List<DiscountCondition> conditions = new();

    public DiscountPolicy(params DiscountCondition[] conditions)
    {
        this.conditions = conditions.ToList();
    }

    public Money CalculateDiscountAmount(Screening screening)
    {
        foreach (var each in conditions)
        {
            if (each.IsSatisfiedBy(screening))
            {
                return GetDiscountAmount(screening);
            }
        }
        return Money.ZERO;
    }

    protected abstract Money GetDiscountAmount(Screening screening);
}
```

- `DiscountPolicy`는 `DiscountCondition`의 리스트인 `conditions`를 가지고 있기 때문에 하나의 할인 정책은 여러 개의 할인 조건을 포함할 수 있다. (다이어 그램에서 1..n의 관계)
- `CalculateDiscountAmount`는 전체 할인 조건에 대해 차례대로 `DiscountCondition`의 `IsSatisfiedBy`메서드를 호출해 할인 조건을 만족하는지 검사한다. 만족하는 조건이 있다면 `GetDiscountAmount`를 호출해 할인 요금을 계산한다.

이 과정은 추상 메서드(동적 바인딩)의 형태로 호출되어 동작한다. 실제로 요금 계산하는 부분은 추상 메서드인 `GetDiscountAmount` 메서드에게 위임한다. 실제 내부는 상속 받은 자식 클래스에서 오버라이딩한 메서드가 실행 될 것이다.

이처럼 부모 클래스에 기본적인 알고리즘의 흐름을 구현하고 중간에 필요한 처리를 자식 클래스에게 위임하는 디자인 패턴을 **템플릿 메서드 패턴**이라고 부른다.

`DiscountCondition`은 인터페이스를 이용하여 선언돼 있다. `IsSatisfiedBy`오퍼레이션은 인자로 전달된 screening이 할인이 가능한 경우 true를 반환하고 그렇지 않은 경우 false를 반환한다.

반환값은 bool이지만 내부 구현은 이를 상속받은 클래스에서 구현할 것이다. 이는 **다형성**의 개념이다.

