## 5장: 설명, 묘사, 논증, 서사로 개발 가이드 쓰기

### 서비스 개념을 범주, 용도, 특징으로 설명하자

#### 범주, 용도, 특징

개발 가이드의 첫 문단은 서비스 개념을 설명하는 자리이기에 제대로 개념을 잡지 못하면 이후에 나오는 내용을 제대로 이해할 수 없다. **이런 서비스 개념을 설명할 때는 범주, 용도, 특징 순으로 쓰는 것이 좋다.** 독자가 잘 아는 범주를 먼저 말하면 독자는 자연스럽게 머릿속으로 해당 범주를 떠올린다.

그다음에 범주의 용도를 말하면 독자는 자연스럽게 서비스의 용도와 같음을 알아차린다. 마지막으로 범주 내의 유사 서비스와 차별화하는 특징을 말하면 독자는 서비스의 개념을 정확히 이해한다.

예시로 네이버 클로바 개발 가이드 문서의 첫 문단이다.

- 범주: Clova는 NAVER가 개발 및 서비스하고 있는 인공지능(AI) 플랫폼입니다.
- 용도: Clova는 사용자의 음성이나 이미지를 인식하고 이를 분석하여 사용자가 원하는 정보나 서비스를 제공합니다.
- 특징: 3rd Party 개발자는 Clova가 가진 기술을 활용하여 인공 지능 서비스를 제공하는 기기 또는 가전제품을 만들거나 보유하고 있는 콘텐츠나 서비스를 Clova를 통해 사용자에게 제공할 수 있습니다.

#### 범주를 정확하고 적절하게 선택하자

개발자는 독자가 이미 가진 범주를 사용함으로써 서비스의 개념을 간단하고 정확히 설명할 수 있다. 여기서 주의할 점은 동물이나 사물의 범주를 가진 단일 용어를 사용하는 것과 달리, 산업이나 서비스의 범주는 유사한 용어가 많다는 것이다.

개발자가 만약 지나치게 트렌드에 앞선 범주를 선택하거나 구체적인 단어를 선택하는 경우 독자는 이해하기 어려워진다. **가장 좋은 방법은 여러 경쟁사가 사용하는 보편적인 서비스 범주를 따라 하는 것이다.**

#### 용도를 범주의 핵심 기능으로 기술하자

첫 문단의 첫 문장이 범주로 적혀있다면 두 번째 문장은 독자 관점의 용도를 적는다. 용도는 독자가 이 서비스를 이용하는 이유다. 즉, 해당 서비스의 핵심 기능을 말한다.

**범주의 핵심 기능을 용도로 설명함으로써 독자의 머릿속에 서비스의 정체성을 명확하게 하는 것이다.** (비유하거나)

#### 특징을 장점과 강점에서 뽑아 쓰자

범주와 용도에는 보편적인 내용을 적는다. 하지만 특징은 차별화하는 내용을 적어야 한다. 개발자에게 차별화는 서비스의 장점과 강점이다. 장점은 자기 기준에서 잘하는 것이고, 강점은 경쟁 서비스와 비교했을 때 뛰어난 것이다.

**장점은 자기 기준인 반면 강점은 시장 기준이다.**

### 정확히 이해하도록 그림과 글로 묘사하자

#### 글에 묘사를 더하면 이해가 빠르다

묘사는 어떤 대상이나 사물, 현상을 언어로 서술하거나 그림을 그려서 표현하는 것이다.

개발문서는 꽤 복잡한 글이다. 이를 잘 이해하기 위해선 묘사가 필요하다. 즉, 묘사가 필요없이 처음부터 **그림을 제공하자.**

#### 글과 그림의 내용을 일치시키자

그림에서는 인공지능 서비스라고 다루고 글에선 클라이언트라고 다룬다면 독자는 혼란스러워한다. **그림과 글의 내용을 일치시키자.** 같은 내용을 다른 용어로 표현하지 말고 같은 용어로 표현하자.

#### 객관적 묘사와 주관적 묘사 둘 다 하자

길을 잃은 사람에게 길을 알려준다고 할 때 주관적 묘사와 객관적 묘사는 다음과 같이 차이가 난다.

- 주관적 묘사: "저기 앞에 큰 사거리에서 오른쪽으로 돌아서 5분쯤 가면 기차역이 보여요"
- 객관적 묘사: "두 번째 사거리에서 10시 방향으로 돌아서 300미터 가면 우측에 기차역이 있어요"

**개발 과정에서 나오는 문서를 보면 주관적 묘사를 많이 하다가 객관적 묘사가 크게 늘고 마지막에는 주관적 묘사가 다시 늘어나는 경향이 있다.**

개발자와 기획자 사이에서 가장 많이 발생하는 문제로 기획자의 주관적 묘사를 개발자는 객관적 묘사로 보며 요구사항 정의서를 만들기 시작한다.

### 논증으로 유용한 정보를 제공하자

#### 의견을 쓰려면 근거를 대자

논증은 옳고 그름을 이유나 근거를 들어서 밝히는 것이다. 논술과 비슷한데, 논술은 논증의 기법으로 기술하는 것이다. 논술의 핵심은 논증인 셈이다. 논증은 객관적이고 논리적인 방식으로 어떤 사실을 증명해서 상대를 설득하는 일이다.

명확한 근거를 제시하는 문서를 작성해라.

#### 거칠게도 공손하게도 쓰지 말자

**개발자가 개발 가이드 문서를 쓰면 은근히 대장 노릇을 할 때가 있다. 자기도 모르게 권력과 힘을 드러내거나 자랑한다. 가장 대표적인 것인 '~할 수 있지만 ~하기 어렵다.'같은 문장이다.**

개발 문서에서 공손한 표현은 좋지 않다. 공손하게 말하는 순간 오해할 소지가 있다. 개발자가 자주 쓰는 공손한 표현에는 다음과 같은 것들이 있다.

- ~해야 합니다만 반드시 해야 하는 것은 아닙니다.
- ~하면 좋습니다. 다만 ~한 경우에는 안 해도 무방합니다.
- ~하지 말아야 합니다. ~한 경우에는 어쩔 수 없으니 넘어가도 됩니다.
- ~하지 마십시오. 물론 큰 문제는 없습니다.
- ~할 것을 추천합니다. 혹시 더 좋은 방법이 있을 수도 있습니다.

이런 표현은 다음과 같이 바꾸자.

- ~하십시오.
- ~하지 마십시오.

세상에는 어떤 일이든 100% 확신할 수 없다. 따라서 독자에게 어떠한 여지도 줘서는 안 된다.

#### 주장과 이유의 거리를 좁혀서 쓰자

주장을 했으면 이유를 바로 이어서 말해야 한다. 앞에서 이유를 말했다고 해서 지금 주장만 하고 이유를 말하지 않으면 안 된다. 특히 개발문서는 잡지와 같아서 처음부터 순서대로 읽지 않는다. **그때그때 필요한 부분을 찾아보기 때문에 주장을 말했으면 중복되더라도 이유를 함께 말하거나 이유를 찾을 수 있는 곳을 알려줘야 한다.**

#### 문제와 답의 거리를 좁혀서 쓰자

논증은 문제 해결과 비슷하다. 논증 자체가 어떤 문제에 대한 특정한 의견이기 때문이다. 개발에서는 문제 하나를 해결하는 방법이 하나밖에 없는 경우는 드물다. 해결 방법이 여러 가지여서 그중 최선을 선택하는 것이 개발 과정이다.

**쉽게 문제와 답에 대한 거리를 줄이고 가는 방법에 대한 내용을 뒤에서 후술한다.**

### 서사를 활용해 목차를 만들자

#### 개발과 서사

서사는 사실을 있는 그대로 순서대로 적는 것을 말한다. 누가 어떤 행동을 했고, 그 행동으로 어떤 일이 일어났는지 그대로 쓰는 것이다. 소설은 대부분 서사 방식으로 쓰여 있다.

개발에서도 서사를 많이 쓴다. 소설과 다른 점은 어떤 일을 하라고 지시한다는 것이다.

많이 사용하는 방법으로 스크린숏안에 번호를 매개 서사를 통해 단계별로 설명하는 방법이 있다.

#### 독자의 수준 대신 기술의 범용성을 기준으로 쓰자

서사가 순서대로 글을 쓰는 것이기는 하지만 단순한 사건이나 상황을 시간순으로 기술하는 것은 아니다. 그것보다는 의미 있는 사건을 시간에 따른 전개 과정으로 써야 한다. 그래야 서사다.

**개발문서에서 서사는 결국 개발자와 독자 사이의 줄다리기 같은 것이다. 개발자는 의미 없는 것은 빼고 중요한 것을 넣기를 원한다. 독자도 의미 있는 것만 남고 무엇이 중요한지 구별해 주기를 원한다.**

**가장 좋은 방법은 기술의 범용성을 기준으로 하는 것이다.** 현재 시대에 맞는 기술 범용성을 이해하고 범용적으로 작성하자. (결국 다른 문서를 참고하자)

#### 순서에서 단계를, 단계에서 목차를 만들자

개발 가이드나 사용법, 도움말 등의 문서에 서사가 많다. 독자가 프로그램을 에러 없이 사용하도록 하기 위해서다. 그런데 어떤 복잡한 것을 순서대로 말하려면 글이 너무 길어진다. 게다가 앞뒤 행동이 연결되는 것도 있지만, 크게 바뀌는 경우도 있다.

만약 25개의 단계가 필요하다면 이를 계속 따라고 하고 있는 독자는 지금 뭘위해서 뭘 하는것인지 전혀 인지하지 못한다. 따라서 단계 개념을 사용한다. 1~5번은 `버그 재현(예시)`이라는 단계를 둬서 자신이 어디에 있는지 알 수 있게 한다.

**각 단계는 반드시 목표가 있어야 한다.** 그래야 단계를 통과할 수 있다. 이런 부류의 문서는 대부분 처음에는 쉽고 뒤로 갈수록 어려워진다. 그래서 단계에서 쉬운 목표를 달성하면 조금씩 어려운 단계의 목표를 달성하게 만들어야 한다.
