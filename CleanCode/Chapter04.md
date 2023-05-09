## 4장 주석

> 주석은 `순수하게 선하지` 못하다.  

사실상 주석은 필요악이다.

주석을 사용하는 이유는 역설적으로 자신의 의도를 코드로 다 표현하지 못해서 사용하는 것이다.  

만약 프로그래밍 언어를 치밀하게 표현할 능력이 있다면 주석은 필요하지 않다..

다른 말로 주석은 실패를 의미한다.  

따라서 주석을 사용하는 상황에선 코드로 표현할 방법이 없는지 다시 한번 생각해봐야 한다.

저자는 주석을 이토록 싫어하는 이유로 거짓말을 하기 때문이라고 한다.

이는 주석 자체는 코드에 영향이 없기 때문에 업데이트되지 않고 유산으로 남기 때문이라고 한다.  

코드가 커지며, 분리되며 부정확한 고아로 변해가는 사례가 너무나도 흔하기 때문에 주석은 거짓말을 한다고 할 수 있다.

그리고 애초에 모든 주석을 실시간으로 전부 업데이트, 추적을 할 수 없는 노릇이기에 없는 주석보다 부정확한 주석은 더욱 나쁘다.  

부정확한 주석은 오히려 독자를 현혹하고, 오용하게 만든다..  

진실은 언제나 하나..! (~~코난~~) 바로 코드이다.  

프로그래머는 코드로만 대화해야 하며 언어 도중에 다른 언어를 혼용할 필요가 없다.  

그러므로 우리는 주석을 줄이는 꾸준한 노력을 해야한다..  

다른 프로그래밍 책에서도 많이 등장하는 파트로 역시 주석의 해악을 다룬다.

### 주석은 나쁜 코드를 보완하지 못한다  

코드에 주석을 추가하는 일반적인 이유는 코드의 품질이 나쁘기 때문이다.

약간 변명의 성격도 가지는 것 같다..?

표현력이 풍부하고 깔끔하며 주석이 거의 없는 코드가, 복잡하고 어수선하며 주석이 많이 달린 코드보다 훨씬 좋다.

> 자신이 저지른 난장판을 주석으로 설명하려 애쓰는 대신에 그 난장판을 깨끗이 치우는 데 시간을 보내라.

### 코드로 의도를 표현하라

코드만으로 의도를 표현하기 어려운 경우가 있다.  

불행히도 많은 개발자가 이를 코드는 훌륭한 수단이 아니라는 의미로 해석한다.

분명히 잘못된 생각이다..

```cs
// 직원에게 복지 혜택을 받을 자격이 있는지 검사한다.
if ((employee.flags & HOURLY_FLAG) && (employee.age > 65))

// 위의 코드를 아래와 같이 바꿔보자.
if (employee.isEligibleForFullBenefits())
```

명확하다.

좀 더 생각을 하면 답이 나오는데 왜..!  

### 좋은 주석

어떤 주석은 필요하거나 유익하다.  

#### 법적인 주석

저작권 정보나 소유권 정보, 소스코드의 유래 등을 표현하는 주석이다.

오픈소스나 라이브러리 정의로 들어가보면 쉽게 볼 수 있다.  

*라이선스에 대한 내용이 많다..*

#### 정보를 제공하는 주석

때로는 기본적인 정보를 주석으로 제공하면 편리하다.  

하지만 함수단위에서 해석이 가능하다.

#### 의도를 설명하는 주석

코드만으로는 의도를 설명하기 어려운 경우가 있다.

그런 경우엔 주석을 사용하여 다른 개발자에게 의도를 설명해야 한다.

#### 의미를 명료하게 밝히는 주석

때때로 모호한 인수나 반환값은 그 의미를 읽기 좋게 표현하면 이해하기 쉬워진다.

일반적으로 인수나 반환값 자체를 명확하게 만들면 더 좋겠지만, 인수나 반환값이 표준 라이브러리나 변경하지 못하는 코드에 속한다면 주석이 유용하다.

#### 결과를 경고하는 주석

때로는 다른 프로그래머에게 결과를 경고할 목적으로 주석을 사용한다.

`C#`의 관점으로는 애트리뷰트로 쉽게 경고할 수 있는 것 같다. ```[Obsolete]```

#### TODO 주석

TODO주석은 프로그래머가 당장 필요하다고 여기지만 당장 구현하기 어려운 업무를 기술한다.  

이 주석은 나도 진짜 많이 사용한다.  

TODO보다는 ```//temp```로 많이 사용하는데 동작을 확인하기 위해서 해당 리터럴 값을 박아두거나 캐싱을 하지 않거나 구조를 잡기 위해서 임시 코딩같은 경우에 //temp로 잡아두고 커밋이 전에 temp를 검색해가며 구조를 다시 잡는다.  

주석보다는 코딩 스타일에 가까운 것 같은데 미리 시각적인 완성을 먼저 보고 고치는걸 선호해서 그런 것 같기도 하다.

좀 더 나아가서 Github Action을 사용해서 Todo주석을 자동으로 Issue로 만들어 주는 기능도 있다..

#### 중요성을 강조하는 주석

자칫 대수롭지 않다고 여겨질 뭔가의 중요성을 강조하기 위해서 주석을 사용한다.

이 부분도 PR단계에서 동료 개발자에게 공유가 되겠지만 매번 추적이 힘들기 때문에 어느정도 중요한 로직에는 주석도 괜찮을 것 같다.

### 나쁜 주석

대다수의 주석이 이 범주에 속한다..!  

일반적으로 대다수 주석은 허술한 코드를 지탱하거나, 엉성한 코드를 변명하거나, 미숙한 결정을 합리화하는 등 프로그래머가 주절거리는 독백에서 크게 벗어나지 못한다.

#### 주절거리는 주석

특별한 이유 없이 의무감으로 혹은 프로세스에서 하라고 하니까 마지못해 주석을 단다면 전적으로 시간낭비다.

2장에서 다룬 이름과 같이 주석또한 오용의 위험이 있기 때문에 그 당시 최고의 주석을 달아야 한다.

*바이트 낭비라니..*

#### 같은 이야기를 중복하는 주석

이 부분은 더블체크에 관한 내용으로 같은 내용의 반복은 오히려 가독성을 떨어트린다..  

비슷한 예제로 `var`키워드로 클래스명 그리고 변수명으로 두번씩 같은 내용을 반복하는 부분이 비슷한 예제라고 생각한다.

한마디로 쓸모없는 짓이다.  

이해가 되지 않는다면 책을 읽는 상상을 해보면 쉽다.  

책을 읽는데 앞에서 읽은 내용이 두번씩 자꾸 반복된다면 책이 이해가 잘 될까? 짜증나서 던져버릴 듯..

#### 오해할 여지가 있는 주석

오해, 오용과 같이 다른 개발자 또는 내가 오해할 만한 주석을 달면 하늘에 쏘아올린 공이 나중에 크게 다가올 수 있다.

#### 의무적으로 다는 주석

의무적으로 주석을 달게 되면 생산성 자체가 매우매우 떨어진다.  

변경할 때 마다 2번씩 작업을 해야하며 오히려 거짓말을 퍼뜨려 혼동과 무질서를 초래한다..

#### 이력을 기록하는 주석

과거에는 좋은 방법이였지만 지금은 킹왕짱 `GIT`을 사용하자.

#### 있으나 마나 한 주석

더블체크와 같은 맥락이다 술취한 사람 처럼 주절거리거나 두번이상 말하거나 있으나 마나 한 이야기를 하지 말자..^^

#### 무서운 잡음

위와 같은 내용

#### 함수나 변수로 표현할 수 있다면 주석을 달지 마라

이 부분을 읽고 든 생각은 좋은 코드, 함수를 짜기 위해선 일단 주석과 같이 글로 적고 주석을 없애보는 훈련도 좋은 방법같다.  

자신의 생각, 로직을 주석으로 적고 주석이 없어질 수 있도록 코드로 설명하는 방법이라면 납득할 수 있지 않을까?

#### 위치를 표시하는 주석

특정 위치를 알려주려는 주석.. 요즘엔 IDE가 하도 좋아져서 이 부분이 의미가 있을까 싶다가 Unity에서 `-------`를 사용해 계층구조에서 구분을 사용했던 기억이 났다.  

그런 부분도 명확하다면 구분이 필요가 없을까..?

#### 닫는 괄호에 다는 주석

이건 처음 보는 주석 스타일.. 나쁜 주석이니까 그냥 넘어가자..

#### 공로를 돌리거나 저자를 표시하는 주석

GIT이 있기 때문에 저자를 표시할 필요가 없다..!!

#### 주석으로 처리한 코드

음.. 아까운 로직은 나는 아직까지 주석으로 남겨둔다.  

따로 깃에 저장하는 경우도 있지만 프로젝트 진행도중 바뀐 로직에 의해 다시 사용할 수도 있기 때문에 주석으로 남겨둔다.

이후에 Tag버전이나 완전히 종료된 경우에 리팩터링 과정에서 삭제하는 것 같다.

#### HTML 주석

> 로버트 마틴: 혐오 그 자체다.

진짜 이 아저씨는 코드 그 자체인듯..

#### 전역 정보

주석을 달아야 한다면 근처에 있는 코드만 기술하라

이 부분은 나는 지금까지 README또한 주석이라 생각했다.

하나의 소프트웨어가 있다면 사용방법에 대한 정도로 생각했는데 조금 다른 문제인 것 같다.

#### 너무 많은 정보

불필요한 정보는 주석으로 달지 말자

#### 모호한 관계

주석과 주석이 설명하는 코드는 둘 사이 관계가 명백해야 한다.

#### 함수 헤더

짧은 함수는 긴 설명이 필요 없다.  

짧고 한 가지만 수행하며 이름을 잘 붙인 함수가 주석으로 헤더를 추가한 함수보다 훨씬 좋다.

### 느낀점

음.. 읽고 생각난 Spine코드를 분석할 때 일인데..  

실제 해당 라이브러리 코드에는 꽤 많은 함수 설명을 위한 주석이 있었다.  

함수의 깊이또한 깊었기 때문에 그걸 다 타고 들어가서 동작 과정을 이해하기엔 무리가 있으니 라이브러리 같은 경우는 사용되는 API에 주석을 달아두는 경우가 많은 것 같다.

같지만 다른 맥락, 다른 상황이라고 생각한다.

#### 논의사항

중간에 나온 `나쁜 주석: 주석으로 처리한 코드`부분에서 저는 리팩터링 이전에는 남겨두는 편인데 같은 작업자가 있다면 안좋은 습관 같습니다..

유산적인 코드들을 남겨놓는 좋은 방법이 있을까요?