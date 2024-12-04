## Item 31: 파일 사이의 컴파일 의존성을 최대로 줄이자

C++을 사용하다 보면 인터페이스가 아닌 구현부를 조금만 수정해도 몽땅 다시 컴파일 되고 다시 링크가 되는 경험을 해본적 있을 것이다. 이 문제의 핵심은 C++은 인터페이스와 구현을 깔끔하게 분리하는 일에 별로 일가견이 없다는데 있다. **C++의 클래스 정의는 클래스 인터페이스만 지정하는 것이 아니라 구현 세부사항까지 상당히 많이 지정하고 있다.**

```cpp
class Person {
public:
    Person(const std::string& name, const Date& birthday, const Address& addr);
    std::string name() const;
    std::string birthDate() const;
    std::string address() const;

    ...

private:
    std::string theName; // 구현 세부 사항
    Date theBirthDate; 
    Address theAddress;
};
```

위 코드만 가지거 `Person` 클래스는 컴파일될 수 없다. Person의 구현 세부사하엥 속하는 것들인 `Date`와 `Address`, `string`가 어떻게 정의 되어 있는지를 모르면 컴파일 자체가 불가능하다.

*string과 같은 표준라이브러리를 사실 문제가 되지 않는다.*

결국 이들이 정의된 정보를 가져 와야 하고, 이때 쓰는 것이 `#include` 지시자이다. 따라서 다음 코드가 추가되어야 한다.

```cpp
#include <string>
#include "Date.h"
#include "Address.h"
```

**바로 이 코드들이 파일 사이의 컴파일 의존성을 가지게 하는 것이다.** 이 상태로 헤더 파일 셋 중 하나라도 바뀌는 순간 이들과 엮어 있는 또 다른 헤더 파일들도 모두 다시 컴파일 되어야 한다. *연결 연결 연결 형태로 무한하게 확장되며 결국 모든 파일이 다시 컴파일 되어야 한다. 이것이 디커플링이 중요한 이유이다.*

전방선언을 사용하여 구현 세부 사항을 따로 지정하면 가능하지 않을까? 리는 생각이 들 수 있다. 이에 대한 문제점은 컴파일 도중 객체들의 크기를 전부 알아야 한다는 점이 있다.

```cpp
int main () {
    int x;

    Person p (params);
}
```

컴파일러 x의 정의문을 만나면 일단 int하나를 담을 충분한 공간을 할당해야 한다. 이 부분은 문제 없이 int는 4바이트라는 표준이 있기에 문제가 되지 않는다. 반면 p의 경우 Person 객체 하나의 크기를 알기 위해서 Person 클래스의 정보를 보는 방법밖에 없다.

*물론 포인터로 실제 객체를 뒤로 숨겨서 사용하는 방버도 있다.*

But, 현대에는 스마터 포인터의 사용과 Modules의 사용이 주가 되어서 `PImpl`과 같은 방법을 사용하지 않는다고 한다.

- 객체 참조자 및 포인터로 충분한 경우에는 객체를 직접 쓰지 않는다.
  - **어떤 타입에 대한 참조자 및 포인터를 정의할 때는 그 타입의 선언부만 필요하다. 반면, 어떤 타입의 객체를 정의할 때는 그 타입의 정의가 준비되어야 한다.**
- 할 수 있으면 클래스 정의 대신 클래스 선언에 최대한 의존하도록 만든다.
  - 어떤 클래스를 사용하는 함수를 선언할 때는 그 클래스의 정의를 가져오지 않아도 된다. 그 클래스 객체를 값으로 전달하거나 반환하더라도 클래스의 정의가 필요없다.
- 선언부와 정의부에 대해 별도의 헤더 파일을 제공한다.
  - 클래스의 선언부와 정의부를 분리하면 클래스의 선언부만을 사용하는 코드는 클래스의 정의부가 변경되어도 다시 컴파일할 필요가 없다.

선언부와 정의부를 구분하는 것은 `C#`에서 사용하는 '인터페이스'와 비슷하지만 C++에는 인터페이스에 대한 명확한 정의가 없기에 실제로 가해지는 제약도 없다. (흉내만 낸다.)

```cpp
class Person {
public:
    virtual ~Person();

    virtual std::string name() const = 0;
    virtual std::string birthDate() const = 0;
    virtual std::string address() const = 0;
    ...
}
```

**이 클래스를 코드에 사용하려면 `Person`에 대한 포인터 혹은 참조자로 프로그래밍하는 방법밖에 없다. 순수 가상 함수를 포함한 클래스를 인스턴스로 만들기는 불가능하다. 또한 인터페이스 클래스의 인터페이스가 수정되지 않는 한 사용자는 다시 컴파일할 필요가 없다.**

또한 인터페이스 클래스를 사용하기 위해서는 객체 생성 수단이 최소한 하나는 있어야 한다. 따라서 대부분 파생 클래스의 생성자 역할을 대신하는 어떤 함수를 만들어 이것을 호출하여 사용한다. (가상 생성자)

**주어진 인터페이스 클래스의 인터페이스를 지원하는 객체를 동적으로 할당 후, 그 객체의 포인터를 반환하는 것이다. 항상은 아니지만 이런 함수는 인터페이스 클래스 내부에 정적 멤버로 선언되는 경우가 있다.**

```cpp
class Person {
public:
    static std::tr1::shared_ptr<Person> create(const std::string& name, const Date& birthday, const Address& addr);

    ...
};

std::string name;
Date birthDate;
Address addr;
...

std::tr1::shared_ptr<Person> pp(Person::create(name, birthDate, addr));
```

이러한 사용이 실질적으로 가능하려면 해당 인터페이스를 지원하는 구체 클래스도 정의가 되어 있어야 하고, 정말로 실행되는 생성자가 호출되어야 한다.

```cpp
class RealPerson : public Person {
public:
    RealPerson(const std::string& name, const Date& birthday, const Address& addr) : theName(name), theBirthDate(birthday), theAddress(addr) {}
    
    virtual ~RealPerson() {}

    std::string name() const { return theName; }
    std::string birthDate() const { return theBirthDate; }
    std::string address() const { return theAddress; }

private:
    std::string theName;
    Date theBirthDate;
    Address theAddress;
};

std::tr1::shared_ptr<Person> Person::create(const std::string& name, const Date& birthday, const Address& addr) {
    return std::tr1::shared_ptr<Person>(new RealPerson(name, birthday, addr));
}
```

### 정리

결론적으로 인터페이스 클래스는 구현부로부터 인터페이스를 떼어냄으로써 파일들 사이의 컴파일 의존성을 완화시키는 효과를 가져다준다. 다만 호출되는 함수들이 전부 가상함수이기 때문에 가상 테이블 점프에 따른 비용이 추가적으로 발생한다.

- 컴파일 의존성을 최소화하는 작업의 배경이 되는 가장 기본적인 아이디어는 '정의' 대신에 '선언'에 의존하게 만들자는 것이다. 이 아이디어에 기반한 두 가지 접근 방법은 핸들 클래스와 인터페이스 클래스이다.
- 라이브러리 헤더는 그 자체로 모든 것을 갖추어야 하며 선언부만 갖고 이는 형태여야 한다.
