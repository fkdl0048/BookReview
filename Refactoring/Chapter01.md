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

