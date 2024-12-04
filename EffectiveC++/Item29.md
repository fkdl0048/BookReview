## Item 29: 예외 안정성이 확보되는 그날 위해 싸우고 또 싸우자!

```cpp
class PrettyMenu {
public:
    ...
    void changeBackground(std::istream& imgSrc);
    ...
private:
    Mutex mutex;
    Image* bgImage;
    int imageChanges;
};
```

```cpp
void PrettyMenu::changeBackground(std::istream& imgSrc)
{
    lock(&mutex);
    delete bgImage;
    ++imageChanges;
    bgImage = new Image(imgSrc);
    unlock(&mutex);
}
```

- [mutex관련 개념](https://github.com/fkdl0048/CodeReview/issues/86)

예외 안정성을 확보하는 작업은 매우 힘든일이다. 위 코드는 예외 안정성측면에서 매우 취약하다. 예외 안정성을 확보하기 위해선 두 가지의 요구사항을 맞춰야 하는데, 이 함수는 어느 요구사항에도 맞지 않는 위험천만한 함수다.

- 자원이 새도록 만들지 않는다.
  - 위 코드는 `new Image(imgSrc)`표현식에서 예외를 던지면 unlock 함수가 실행되지 않게 되어 뮤텍스가 계속 잡힌 상태로 남기 때문이다.
- 자료구조가 더렵혀지는 것을 허용하지 않는다.
  - `imgsrc`가 예외를 던지면 bgImage가 가리키는 객체는 이미 삭제된 상태이다.

자원 누출의 문제는 사실 이미 다룬 [Item13](https://github.com/fkdl0048/BookReview/issues/295)과 [Item14](https://github.com/fkdl0048/BookReview/issues/296)에서도 다루었던 내용이기에 해당 아이디어로 해결할 수 있다. *최근에는 [Rule of Zero](https://github.com/fkdl0048/CodeReview/issues/72)를 따르는 것이 좋다.*

### 자료구조 오염 문제

앞서 Lock 자원관리 전담 클래스를 두어 해결했다면(자원 관리 클래스를 두어 코드 스코프 내에 C++의 특성인 생성자와 소멸자를 이용해 자동으로 자원을 관리하는 방법) 이번에는 **자료구조 오염** 문제가 있다.

그전에 예외 안전성을 갖춘 함수는 아래 세 가지 보장 증 하나를 제공한다.

- **기본적인 보장(basic guarantee)**
  - 함수 동작 중에 예외가 발생하면, 실행 중인 프로그램에 관련된 모든 것들을 유효한 상태로 유지하겠다는 보장이다. 어떤 객체나 자료구조도 더렵혀지지 않으며, 모든 객체의 상태는 내부적으로 일관성을 유지하고 있다. (즉, 모든 클래스 불변속성이 만족된 상태, 비트상수성이 만족된 상태)
- **강력한 보장(strong guarantee)**
  - 함수 동작 중에 예외가 발생하면, 프로그램의 상태를 절대로 변경하지 않겠다는 보장이다. 이런 함수를 호출하는 것은 원자적인(atomic) 동작이라고 할 수 있다. 호출이 성공하면(예외가 발생하지 않으면) 마무리까지 완벽하게 성공하고, 실패하면 함수 호출이 없었던 것처럼 원래 상태로 복구한다.
- **예외불가 보장(nothrow guarantee)**
  - 예외를 절대로 던지지 않겠다는 보장이다. 약속한 동작은 언제나 끝까지 완수하는 함수라는 뜻이다. 기본 제공 타입에 대한 모든 연산은 예외를 던지지 않게 되어 있다. (즉, 예외불가 보장이 제공된다.) 예외에 안전한 코드를 만들기 위한 가장 기본적이며 핵심적인 요소이다.

**앞서 말한 예외 안전성을 갖춘 함수는 위의 세 가지 보장 중 하나를 반드시 제공해야 한다. 아무 보장도 제공하지 않으면 예외에 안전한 함수가 아니다.** 