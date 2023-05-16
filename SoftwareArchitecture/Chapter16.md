## 16. 오케스트레이션 기반 서비스 지향 아키텍처 스타일

*이름도 어렵네..*

> 아키텍처 스타일은 예술 활동처럼 그것이 진화한 시대의 맥락에서 이해해야 한다.

### 16.1. 역사와 철학

서비스 지향 아키텍처는 1990년대 후반에 등장했고, 매우 빠른 속도로 성장했다.

하지만 이런 성장을 하드웨어 장비가 따라가지 못했기 때문에 가변적인 확장성, 다른 유용한 특성을 요구했다.

이 시대의 아키텍트들은 다양한 외부 여건 탓에 어쩔 수 없이 제약이 많은 분산 아키텍처를 구축했다.

오픈 소스 운영 체제를 프로덕션에서 사용해도 좋을 만큼 신뢰감을 얻기 전이었으므로 운영 체제는 많은 비용을 들여 시스템 단위로 라이센스를 구매했다.

*사용법이 복잡*

그 결과 아키텍트는 최대한 재사용하는 것을 목표로 삼게 되었고, 실제로 모든 형태의 재사용은 이 아키텍처의 중심 철학이 되었다.

### 16.2. 토폴로지

> 이 아키텍처 스타일은 아키텍트가 기술 분할에 집착하면 어떻게 되는지 잘 보여주는 사례이다.

아키텍처 내부에 서비스 택소노미를 정립하여 레이어별로 책임을 지운다는 아이디어는 동일하다.

서비스 지향 아키텍처는 분산 아키텍처이다. 분산 아키텍처는 조직마다 다양하다.

### 16.3. 택소노미

이 아키텍처의 중심 철학은 엔터프라이즈 레벨의 재사용이다.

#### 16.3.1. 비즈니스 서비스

이 아키텍처 최상단에 위치한 비즈니스 서비스는 진입점 역할을 한다.

이 서비스 정의는 코드는 전혀 없고, 입력, 출력, 스키마 정보만 갖고 있다.

서비스는 대부분 비즈니스 유저가 정의하기 때문에 이름도 비즈니스 서비스이다.

#### 16.3.2. 엔터프라이즈 서비스

엔터프라이즈 서비스는 세분화된 공유 구현체를 포함한다.

일반적으로 개발팀은 특정 비즈니스 도메인에 관한 원자적 행위를 구현하는 업무를 담당한다.

이런 서비스는 오케스트레이션 엔진을 통해 묶인, 단위가 큰 비즈니스 서비스를 구성하는 요소들이다.

이 아키텍처의 재사용 목표 때문에 이렇게 책임을 분리한 것이다.

개발자가 정확한 세분도에 맞게 서비스를 구축할 수 있다면 비즈니스 워크플로의 해당 파트를 재작성할 필요가 없기 때문..

그러나 현실에서는 실패했다.

#### 16.3.3. 애플리케이션 서비스

아키텍처의 모든 서비스에서 엔터프라이즈 서비스와 동일한 레벨의 세분화 또는 재사용이 필요한 것은 아니다.

애플리케이션 서비스는 한 번만 사용 가능한 단일 구현체 서비스이다.

#### 16.3.4. 인프라 서비스

인프라 서비스는 모니터링, 로깅, 인증/인가 등의 운영 관심사를 지원한다.

이런 서비스는 운영팀과 긴밀하게 협업하는 공유 인프라팀이 소유한 실질적인 구현체인 경우가 많다.

#### 16.3.5. 오케스트레이션 엔진

오케스트레이션 엔진은 이 분산 아키텍처의 요체이다.

이 엔진은 비즈니스 서비스 구현체를 서로 엮어주며 트랜잭션 조정과 메시지 변환 등의 기능을 수행한다.

마이크로 서비스 아키텍처처럼 서비스마다 데이터베이스가 있는 것은 아니며, 일반적으로 단일 관계형 데이터 베이스를 사용한다.

따라서 트랜잭셔널 로직은 데이터베이스가 아닌 오케스트레이션 엔진에서 선언적으로 처리된다.

#### 16.3.6 메시지 흐름

모든 요청은 오케스트레이션 엔진을 흘러간다.

### 16.4. 재사용... 그리고 커플링

이 아키텍처의 주된 목표는 서비스 레벨의 재사용, 즉 시간이 지남에 따라 재사용이 가능한 비즈니스 행위를 점직적으로 구축하는 능력이다.

### 16.5. 아키텍처 특성 등급

서비스 지향 아키텍처는 어쩌면 지금까지 시도된 아키텍처 중에서 가장 기술적으로 분할된 범용 아키텍처일 것이다.

모놀리식과 분산 아키텍처 모두의 단점을 갖고 있다.

### 느낀점

어떻게 보면 최고의 아키텍처를 만들려다 실패한 사례를 보여줌으로 트레이드 오프를 간과하지 말라는 말을 전하고 싶었던게 아닐까?

#### 논의사항

없습니다..