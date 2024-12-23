## Item 18: 인터페이스 설계는 제대로 쓰기엔 쉽게, 엉터리로 쓰기엔 어렵게 하자

C++에서는 대부분이 인터페이스이다. 함수, 클래스, 템플릿 모두.. *구현을 강제하는 인터페이스를 말하는 것은 아니지만, 사실 그것도 결국 인터페이스이다.*

이상적으로는, 어떤 인터페이스를 어떻게 써 봤는데 결과 코드가 사용자가 생각한 대로 동작하지 않는다면 그 코드는 컴파일되지 않아야 맞다. 거꾸로 생각해서, 어떤 코드가 컴파일된다면 그 코드는 사용자가 원하는대로 동작해야 할 것이다.

*명확한 인터페이스를 제공하는 것은 중요하다. 이는 꼭 코드에서만 해당하는 것이 아닌 일상에서도 청소기나 컴퓨터, 키오스크 모두 해당하는 말이다.*

*좀 더 개인적인 생각을 말해본다면, 제대로 쓰기엔 쉽게가 절대 섵부른 최적화나 사용하지 않을 기능까지 개발하면 안된다고 생각한다. 정확하게 해당 인터페이스 성격과 역할에 맞는 기능을 수행할 수 있도록 해야한다.*

*반대로 너무 큰 자유도도 문제가 되기에 어느정도 제한점과 실제 처리하는 영역에서의 제한점을 두는 경우가 더 좋은 때도 있다. 정수값을 특정값으로 제한할 수 있도록 Clamp를 사용하거나, 디폴트 매개변수를 지정해두는 경우도 여기에 해당한다. 자유도와 제한점의 트레이드 오프가 중요한데, 그 중간점은 아마도 동작해야 하는 기능과 편의성에 해당될 것 같다.*

```cpp
class Date {
public:
    Date(int month, int day, int year);
    ...
};

Date d(1, 2, 3);
```

이런 코드는 코딩테스트에서나 적합한 코드로, 실제로 사용하는 함수로는 매우 위험하다. 이를 해결하기 위해 새로운 타입을 설정하여 (즉, 타입으로 제한한다. 추상화와 인터페이스의 개념) 컴파일러가 오류를 잡아낼 수 있도록 하는 것이 좋다.

```cpp
struct Day {
    struct Month {
        ...
    }

}

class Date {
public:
    Date(const Month& m, const Day& d, const Year& y);
    ...
};

Date d(Month::Jan(), Day(31), Year(1995));
```

*class enum이라는 더 안정한 열거형도 있다.*

### 타입이 제약을 부여하여 그 타입을 통해 할 수 있는 일을 묶어 버리기

제약을 부여 방법으로 아주 흔히 쓰이는 `const`붙이기 이다. [Item3](https://github.com/fkdl0048/BookReview/issues/279)에서 잘 설명이 되어 있는데, operator *의 반환 타입을 const로 한정함으로써 사용자가 사용자 정의 타입에 대한 실수를 저지르지 않도록 할 수 있다.

**제대로 쓰기에 괜찮은 인터페이스를 만들어 주는 요인 중에 일관성만큼 똑 부러지는 것이 별로 없으며, 편찮은 인터페이스를 만들어 주는 요인 중에 비일관성을 따라오는 것이 거의 없다.** 가장 큰 예로 STL이 가지고 있는 일관성 덕분에 사용자는 예측하고 예상할 수 있는 것이다.

*내 생각엔 추가로 개발자의 보편성도 어느정도 들어가야 한다고 생각한다. 당연히 컨벤션이라는 개념도 있지만 보이지 않는 보편성도 괜찮은 인터페이스 설계에 해당된다. 개발자의 보편성도 좋지만 해당 팀의 보편성도 중요하다.*

앞서 다룬 createInvestment 함수또한 포인터를 반환하기보다 스마터 포인터로 제한을 하는 방식을 사용해야 한다. *현대에는 대부분 이렇게 사용한다.*

### 정리

- 좋은 인터페이스는 제대로 쓰기에 쉬우며 엉터리로 쓰기에 어렵다. 인터페이스를 만들때는 이 특성을 지닐 수 있도록 고민하자.
- 인터페이스의 올바른 사용을 이끄는 방법으로는 인터페이스 사이의 일관성을 잡아주기, 그리고 기본 제공 타입과의 동작 호환성을 유지하기가 있다.
- 사용자의 실수를 방지하는 방법으로는 새로운 타입 만들기, 타입에 대한 연산을 제한하기, 객체의 값에 대해 제약 걸기, 자원 관리 작업을 사용자 책임으로 놓지 않기가 있다.
- `shared_ptr`은 사용자 정의 삭제자를 지원한다. 이 특징 때문에 교차 DLL 문제를 막아주며 뮤텍스 등을 자동으로 잠금해제하는데 쓸 수 있다.
