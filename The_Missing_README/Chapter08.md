## 8장 고객 앞으로! 소프트웨어 전달

> 마침내 프로덕션 환경에 안착시킬 소프트웨어의 종착지

실제 고객에게 소프트웨어가 전달되는 흐름을 이해하고 제작하는 것은 전대 다르다. 소프트웨어 전달은 릴리스, 배포, 롤아웃 등의 단계를 거치며 그 과정에서 소스코드 컨트롤 전략 등을 알아본다.

### 소프트웨어 전달의 4가지 단계

소프트웨어 전달의 단계는 업계에서 정해진 표준이 없으며 기업이나 프로젝트에 따라 다를 수 있다. 하지만 크게 빌드, 릴리스, 배포, 롤아웃 단계로 나눌 수 있다.

소프트웨어는 반드시 패키지로 빌드돼야한다. 패키지는 반드시 불변이며 버전이 지정돼야 한다. 또한 패키지는 생성되면 반드시 릴리스돼야 하며, 릴리스 과정에서 릴리스 노트와 변경 로그도 업데이트한다. 그 다음 중앙식 레포로 발행한다.

발행된 릴리스 산출물은 반드시 테스트 환경이나 프로덕션 환경에서 배포돼야 하며 배포된 소프트웨어는 아직까지는 사용자가 접근할 수 없다. 단지 지금 막 인스톨됐을 뿐이다.

*이러한 과정으로 흘러간다는 흐름만 이해해도 될 것 같다. 실제 실무를 겪고 나면 다르게 보일 것*

### 효과적인 버전 제어를 위한 브랜치 전략

책에서 나오는 트렁크도 있지만, 대부분 깃 플로우나 깃허브 플로우를 사용한다. 이 두가지 방법은 브랜치 전략을 사용한다.

자주 머지하는 방식을 CI(지속적 통합) 브랜치 전략이라고 한다. **이 전략은 각 개발자가 너무 오래 최신 버전의 코드로부터 벗어나는 일이 적어지므로 위험을 줄일 수 있다.** 개발자 간에 코드베이스가 잘 동기화돼 있으면 마지막 순간에 통합하는 과정에서의 장애물을 피할 수 있으며 버그나 호환성 문제도 일찍 발견할 수 있다.

### 빌드 단계

소프트웨어를 전달하기에 앞서 반드시 먼저 패키지를 빌드해야 한다. 소프트웨어 빌드는 의존성 해석 및 링킹, 런터 실행, 컴파일, 테스트, 소프트웨어 패키징 등 여러 단계로 구성된다.

#### 패키지에 버전을 명시하자

패키지에는 버전을 명시해야 하며 고유한 식별자를 할당해야 한다. 고유한 식별자를 할당해두면 운영자와 개발자는 실행중인 애플리케이션의 특정 소스 코드, 기능, 문서와 엮을 수 있다.

#### 리소스는 각각 별도로 패키징하자

소프트웨어는 단순히 코드로만 구성되지 않는다. 설정, 스키마, 이미지 언어팩도 소프트웨어의 일부다. 각각 리소스는 서로 다른 릴리스 방식을 가지고 다른 검증이 필요하다. 게임은 더 다양한 레포를 필요로 할 것 같다..

### 릴리스 단계

사용자가 소프트웨어를 사용할 수 있게 하는 릴리스는 소프트웨어 전달의 다음 단계인 배포를 위한 단계다. 릴리스는 소프트웨어의 종류와 크기 및 사용자의 숙련도에 따라 다양한 방법으로 수행한다.

**릴리스 관리는 안정적이며 문서화가 잘된 소프트웨어를 예측 가능한 시점에 발행하는 기술이다.** 깃허브에서도 이런 기능을 지원하기에 사용하면 좋다.

#### 릴리스를 남의 일로 여기지 말자

소프트웨어 릴리스에 대한 책임 의식을 가지고 있어야 한다. 조직에 있으면 좋지만 없더라도 릴리스는 다양한 이유, 위와 같은 이유로 중요하다.

#### 패키지를 릴리스 레포로 발행하자

릴리스 패키지는 대부분 패키지 레포에서 발행하거나 깃 같은 버전 제어 시스템에 간단히 태그로 생성하기도 한다. 두 방법 다 괜찮지만 가능하다면 릴리스 패키지는 전용 패키지 레포에서 발행하는 편이 좋다.

실제 오픈소스중에는 릴리스 레포만 따로 파고 개발은 다른 곳에서 진행하는 경우가 종종 있다.

#### 릴리스는 불변성을 갖게 하자

**일단 릴리스 패키지를 발행했다면 절대 변경하거나 덮어쓰지 말자.** 불변 릴리스는 동일한 버전을 실행하는 모든 애플리케이션 인스턴스가 완전히 동일한 코드를 실행하도록 보장해준다.

릴리스 패키지가 동일하면 개발자는 애플리케이션의 어떤 코드를 실행하며 어떻게 동작해야 하는지를 유추할 수 있다. 버전이 할당된 패키지가 바뀌는 것은 패키지에 버전을 할당하지 않는 것과 매한가지다.

#### 자주 릴리스하자

릴리스는 최대한 자주 수행하자. 릴리스가 주기가 늘어지면 **거짓된 안정감**을 심어줄 수 있다. 릴리스 사이의 주기가 길면 변경사항을 테스트할 충분한 시간이 있다고 착각하기 때문에 간격을 줄이면 버그를 좀 더 쉽게 처리할 수 있다.

*그렇다고 너무 자주도 별로일 것 같다. 마일스톤에 맞게 릴리스를 하는 것이 좋을 것 같다.*

#### 릴리스 일정은 투명하게 공유하자

릴리스 일정은 말 그대로 소프트웨어 일리스 빈도를 결정한다. 예측 가능한 기준으로 해서 매 분기나 매년 릴리스를 수행하는 프로젝트도 있다. 책에서 말하는 마일스톤을 릴리스로 잡는 것을 생각해보자. *팀원 모두에게 알려줘야 할 것*

#### 변경 로그와 릴리스 노트를 발행하자

변경 로그와 릴리스 노트는 릴리스에 어떤 변경사항이 반영됐는지를 사용자와 고객지원팀이 이해하는 데 도움이 된다. 로그 자체로 문서화가 되고 어떤 변경사항이 발생했는지 알 수 있게 해준다. 스팀에서 제공하는 패키지 노트와 비슷한 것 같다.

### 배포 단계

소프트웨어 배포란 패키지를 실행할 곳으로 옮기는 것이다. 배포 메커니즘은 다양하지만 기본원리는 같다.

#### 배포를 자동화하자

소프트웨어 배포는 직접 수행하지 말고 스크립트를 이용하자. 자동화를 통해 배포를 반복하면 버전 제어, 예측 가능하게 하거나 실수자체가 방지된다. 이 과정을 CI/CD로 자동화하면 더 좋다. (github Action 참고) 게임에서는 젠킨스를 많이 사용하는 듯하다.

**자동화를 고도화하면 지속적 전달(CD)로 이어진다. 지속적 전달을 적용하면 사람은 배포 절차에서 완전히 배재된다. 패키징, 테스팅, 릴리스, 배포, 심지어 롤아웃까지 모두 자동화할 수 있다.**

#### 배포는 원자적으로 수행하자

인스톨 스크립트는 여러 단계로 구성되는 경우가 많다. 따라서 매번 실행할 때마다 모든 단계가 성공할 것이라고 간주하지 말자. 머신의 디스크 용량이 부족해지거나 엉뚱한 시간에 재시작하거나 또는 예상치 못한 파일 권한 오류가 발생할 수 있다.

다른 의존 관계 없이 배포는 원자적으로 수행되어야 문제가 생기지 않는다.

### 롤아웃 단계

새 코드가 배포되고 나면 이제 그 코드를 동작시킬 수 있다. 한 번에 모조리 새 코드로 전환하는 일은 위험하다. 테스트를 아무리 많이 해도 버그 발생 가능성을 없앨 수는 없으며 한 번에 모든 사용자에게 코드를 롤아웃하면 모두가 동시에 문제를 겪을 수 있다.

롤아웃 전략은 기능 플래그, 서킷 브레이커, 다크 론치, 카나리 배포, 블루-그린 배포등 여러 가지가 있다.

- 기능 플래그: 코드 경로를 실행하는 사용자의 비율을 제어할 수 있다
- 서킷 브레이커: 문제가 발생하면 자동으로 코드 경로를 바꿔준다.

*너무 복잡한 롤아웃 전략은 오히려 운영 복잡도만 증가할 뿐이다. 그래서 관리자가 따로 필요한 이유가 아닐까?*

#### 롤아웃을 모니터링하자

새 코드를 활성화할 때는 에러율, 응답 시간, 리소스 사용량 등 상태 지표를 모니터링하자. 통계값과 롤아웃가정을 반드시 관찰해야 하며 범위를 늘리거나 줄이는 것은 로그와 지표를 관찰하는 사람이 결정하는 경우가 보편적이다.

**코드는 커밋한다고 끝이 아니며 코드가 롤아웃된 이후에도 여전히 끝난 것이 아님을 기억하라!**

#### 기능 플래그를 활용하자

기능 플래그(feature flag)를 이용하면 개발자는 언제 새로운 코드를 사용자에게 릴리스할지를 결정할 수 있다. 코드가 플래그를 확인해서 특정 코드를 실행할지 여부를 결정하는 if 구문으로 감싸져 있다.

```python
if feature_flags['new_game_mode']:
    # 새로운 게임 모드를 활성화하는 코드
else:
    # 기존 게임 모드를 유지하는 코드
```

#### 서킷 브레이커를 이용해 코드를 보호하자

대부분 기능 플래그는 사람이 제어한다. 반면 서킷 브레이커는 레이턴시나 예외 같은 운영 이벤트에 의해 자동으로 제어되는 **기능 플래그**다. 서킷 브레이커는 간단히 토글되며 영구적이고 자동화됐다는 독특한 특징이 있다.

*아마 게임쪽에선 서버 관련해서 특정 서킷 브레이커를 많이 사용할 것 같다.*

#### 서비스 버전은 병렬로 올리자

웹 서비스는 기존 버전을 계속 실행하면서 새로운 버전을 배포하는 것이 가능하다. 이런 경우 패키지는 동일한 머신에 함께 배포할 수도 있고 완전히 새로운 하드웨어에 배포할 수도 있다. 병렬 배포를 이용하면 천천히 버전을 올리면서 위험을 완화할 수 있으며 뭔가 잘못된 경우 신속하게 롤백할 수도 있다.

*책에서 말하는 블루그린, 카나리 등을 게임에 덧대어 조금 쉽게 말하면 테스트 버전과 실제 버전을 병렬로 운영하는 것 같다.*