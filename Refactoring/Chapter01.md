## 1. 리팩터링: 첫 번째 예시

> 원칙은 지나치게 일반화되기 쉬워서 실제 적용 방법을 파악하기 어렵지만 예시가 있으면 모든 게 명확해진다.  

1장은 앞 서 말한대로 리팩터링의 예시를 보여주는 장이다.

### 1.1 자, 시작해보자!

- 다양한 연극을 외주로 받아서 공연하는 극단이 있다.
- 공연 요청이 들어오면 연극의 장르와 관객 규모를 기초로 비용을 책정한다.
- 현재 이 극단은 두 가지 장르, 비극과 희극만 공연한다.
- 공연료와 별개로 포인트를 지급해서 다음번 의뢰 시 공연료를 할인받을 수도 있다.(충성도 시스템)

극단은 공연할 연극 정보를 다음과 같이 간단한 JSON 파일에 저장한다.

```json
// plays.json
{
    "hamlet": {"name": "Hamlet", "type": "tragedy"},
    "as-like": {"name": "As You Like It", "type": "comedy"},
    "othello": {"name": "Othello", "type": "tragedy"}
}
```

공연료 청구성에 들어갈 데이터도 다음과 같이 JSON 파일에 저장한다.

```json
// invoices.json
[
    {
        "customer": "BigCo",
        "performances": [
            {
                "playID": "hamlet",
                "audience": 55
            },
            {
                "playID": "as-like",
                "audience": 35
            },
            {
                "playID": "othello",
                "audience": 40
            }
        ]
    }
]
```

공연료 청구서를 출력하는 코드는 다음과 같다.

```javascript
// function statement

function statement(invoice, plays) {
    let totalAmount = 0;
    let volumeCredits = 0;
    let result = `청구 내역 (고객명: ${invoice.customer})\n`;
    const format = new Intl.NumberFormat("en-US", {
        style: "currency", currency: "USD",
        minimumFractionDigits: 2
    }).format;

    for (let perf of invoice.performances) {
        const play = plays[perf.playID];
        let thisAmount = 0;

        switch (play.type) {
            case "tragedy": // 비극
                thisAmount = 40000;
                if (perf.audience > 30) {
                    thisAmount += 1000 * (perf.audience - 30);
                }
                break;
            case "comedy": // 희극
                thisAmount = 30000;
                if (perf.audience > 20) {
                    thisAmount += 10000 + 500 * (perf.audience - 20);
                }
                thisAmount += 300 * perf.audience;
                break;
            default:
                throw new Error(`알 수 없는 장르: ${play.type}`);
        }

        // 포인트를 적립한다.
        volumeCredits += Math.max(perf.audience - 30, 0);
        // 희극 관객 5명마다 추가 포인트를 제공한다.
        if ("comedy" === play.type) volumeCredits += Math.floor(perf.audience / 5);

        // 청구 내역을 출력한다.
        result += `${play.name}: ${format(thisAmount / 100)} (${perf.audience}석)\n`;
        totalAmount += thisAmount;
    }
    result += `총액: ${format(totalAmount / 100)}\n`;
    result += `적립 포인트: ${volumeCredits}점\n`;
    return result;
}
```

자바 스크립트 코드도 어렵지 않게 이해가 되지만 좀 더 가독성 좋은 코드를 위해 구조를 수정하지 않고 `C#`으로 바꿔서 작성하였다.

```csharp
public class Play
{
    public string name;
    public string type;

    public Play(string name, string type)
    {
        this.name = name;
        this.type = type;
    }
}
```

```csharp
public class Performance
{
    public string playID;
    public int audience;

    public Performance(string playID, int audience)
    {
        this.playID = playID;
        this.audience = audience;
    }
}
```

```csharp
public class Invoice
{
    public string customer;
    public List<Performance> performances;

    public Invoice(string customer, List<Performance> performances)
    {
        this.customer = customer;
        this.performances = performances;
    }
}
```

```csharp
public class Account
{
    public String Statment(Invoice invoice, List<Play> plays)
    {
        int totalAmount = 0;
        int volumeCredits = 0;
        string result = $"청구 내역 (고객명:{invoice.customer})\n";

        foreach (var perf in invoice.performances)
        {
            var play = plays.FirstOrDefault(p => p.name == perf.playID);
            int thisAmount = 0;

            switch (play.type)
            {
                case "tragedy":
                    thisAmount = 40000;
                    if (perf.audience > 30)
                    {
                        thisAmount += 1000 * (perf.audience - 30);
                    }

                    break;
                case "comedy":
                    thisAmount = 30000;
                    if (perf.audience > 20)
                    {
                        thisAmount += 10000 + 500 * (perf.audience - 20);
                    }

                    thisAmount += 300 * perf.audience;
                    break;
                default:
                    throw new Exception($"알 수 없는 장르:{play.type}");
            }

            // 포인트를 적립한다.
            volumeCredits += Math.Max(perf.audience - 30, 0);
            // 희극 관객 5명마다 추가 포인트를 제공한다.
            if ("comedy" == play.type) volumeCredits += (int) Math.Floor((decimal) perf.audience / 5);

            // 청구 내역을 출력한다.
            result += $"{play.name}: ${thisAmount / 100} ({perf.audience}석)\n";
            totalAmount += thisAmount;
        }

        result += $"총액: ${totalAmount / 100}\n";
        result += $"적립 포인트: {volumeCredits}점\n";

        return result;
    }
}
```

```csharp
public class Program
{
    public static void Main()
    {
        List<Play> plays = new List<Play>();
        
        plays.Add(new Play("Hamlet", "tragedy"));
        plays.Add(new Play("As You Like It", "comedy"));
        plays.Add(new Play("Othello", "tragedy"));

        List<Performance> performances = new List<Performance>();

        performances.Add(new Performance("Hamlet", 55));
        performances.Add(new Performance("As You Like It", 35));
        performances.Add(new Performance("Othello", 40));

        Invoice invoices = new Invoice("BigCo", performances);

        Account account = new Account();
        Console.WriteLine(account.Statment(invoices, plays));
    }
}
```

![image](https://github.com/fkdl0048/ToDo/assets/84510455/90d1a10e-6b29-45ca-bfc8-92e9313d4795)

*json utill까지 설치하고 직렬화 과정까진 하지 않고 객체를 생성하여 동작만 동일하게 만들었다.*

정확한 동작을 실제로 보고 싶다면 [CodeReview](https://github.com/fkdl0048/CodeReview/tree/main/Refactoring)에서 확인할 수 있다.

뒤에서 등장하는 리팩터링 과정또한 해당 코드로 진행하며 진행과정 또한 커밋으로 기록할 예정

### 1.2 예시 프로그램을 본 소감

저자와 마찬가지로 코드가 짧아서 그런지 어렵지 않게 이해할 수 있었다.

다만 막히는 부분은 본문에 없던 도메인, 요구사항이 수식으로 나오는 부분을 제외하고 동작 자체는 크게 어려움 없었다.

과연 앞 장에서도 다룬 코드는 동작하기만 하면 문제가 없는 것일까?

컴파일러는 코드가 깔끔하든 지저분하든 상관없이 동작하며 대부분 사람이 문제이다.

사람은 코드를 눈으로 보고 풀이해야 하므로 가독성과 미적인 기준은 상당히 중요하다.

*코드가 실행안될 때, 버그가 많을 때 위안이 되는 말 중 하나로 "하나는 컴퓨터는 거짓말을 하지 않는다"*

저자는 수백 줄짜리 코드를 수정할 때면 먼저 프로그램의 작동 방식을 더 쉽게 파악할 수 있도록 코드를 여러 함수와 프로그램 요소로 재구성한다.

프로그램의 구조가 빈약하다면 대체로 구조부터 바로잡은 뒤에 기능을 수정하는 편이 작업하기가 훨씬 수월하다.

> 프로그램이 새로운 기능을 추가하기에 편한 구조가 아니라면, 먼저 기능을 추가하기 쉬운 형태로 리팩터링하고 나서 원하는 기능을 추가한다.

이제 코드를 수정할 부분을 살펴보자

#### HTML 출력 기능 추가하기

사용자의 요청에 따라 HTML 출력 기능이 필요하다.

HTML태크를 삽입해야 하니 문자열을 추가하는 각각을 조건문으로 감싸야 한다.

함수 내부에 조건문을 사용하여 구분하면 Statement함수의 복잡도가 크게 증가한다.

이런 상황이라면 대부분 이 함수의 복사본을 만들고 복사본에서 HTML을 출력할 것이다.

이 때 복사하는 일 자체는 큰 부담이 없지만 나중에 문제를 야기할 수 있다.

청구서 작성 로직을 변경할 때 마다 기존 함수와 HTML버전 함수 모두를 수정해야 하고, 항시 일관된지 수정해야 한다.

즉, 중복에 의한 사이드 이펙트가 발생할 확률이 매우 높다.

이는 두 번째 요구사항과도 관련이 있다.

#### 다양한 장르의 추가

희극과 비극뿐만 아니라 다양한 장르의 연극을 공연하게 될 수도 있다.

이런 장르가 늘어남에 따라 예측할 수 없을 때 조건문을 추가해야 한다면 이후에는 폭탄해체 작업을 해야할 수 있다..

> 새로운 요구사항은 수색 대원처럼 한두 명씩이 아니라, 한 부대씩 몰려오기 마련이다.

이처럼 연극 장르와 공연료 정책이 달라질 때마다 statement()함수를 수정해야 한다.

바로 이러한 변경 사항들 때문에 리팩터링이 필요하다.

### 1.3 리팩터링의 첫 단계

