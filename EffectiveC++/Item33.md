## Item 33: 상속된 이름을 숨기는 일은 피하자

```cpp
int x;

void SomeFunc() {
    double x;
    
    std::cin >> x; // double x를 사용
}
```

위 코드에서 값을 읽어 x에 넣는 위의 문장에서 실제로 참조하는 x는 전역 변수 x가 아닌 지역 변수 x이다. 이유는 안쪽 유효범위에 있는 이름이 바깥쪽 유효범위에 있는 이름을 가리기 때문이다. *다른 언어도 동일함*

컴파일러가 `someFunc`의 유효범위 안에서 x라는 이름을 만나면, 일단 그 컴파일러는 자신이 처리하고 있는 유효범위, 즉 지역 유효범위를 탐색하여 같은 이름을 찾는다. 위 코드대로 `double x`가 있기 때문에 더 이상 탐색하지 않는다.

클래스로 넘어가서 기본 클래스에 속해 있는 것(멤버 함수, 데이터 멤버)을 파생 클래스 멤버 함수 안에서 참조하는 문장이 있으면 컴파일러는 이 참조 대상을 바로 찾아낼 수 있다. 이는 기본 클래스에 선언된 것은 파생 클래스가 모두 물려받기 때문이다.

```cpp
class Base {
private:
    int x;
public:
    virtual void mf1() = 0;
    virtual void mf2();
    void mf3();
};

class Derived : public Base {
public:
    virtual void mf1();
    void mf4();
};
```

이러한 클래스가 있다고 가정하고 파생 클래스의 mf4함수가 다음 과 같이 구현되어 있다.

```cpp
void Derived::mf4() {
    ...
    mf2();
}
```

컴파일러는 이 함수 안을 차례대로 읽다가 mf2라는 이름을 쓰이고 있다는 것을 알고 유효범위에 맞게 탐색을 시작한다. 첫 번째 유효 범위는 `mf4()`함수 내부에서 시작하고 없기 때문에, 다음 유효 범위로 넘어가면 Derived 클래스의 유효 범위가 된다. 이 클래스에는 mf2가 없기 때문에 다시 상위 클래스인 Base 클래스로 넘어가서 mf2를 찾는다. 만약 여기도 없다면 Base를 둘러싸고 있는 네임스페이스로 시작하여 마지막엔 전역 유효 범위까지 찾는다.

이번엔 좀 다르게 오버로딩을 섞은 예제를 보자.

```cpp
class Base {
private:
    int x;
public:
    virtual void mf1() = 0;
    virtual void mf1(int);
    virtual void mf2();
    void mf3();
    void mf3(double);
};

class Derived : public Base {
public:
    virtual void mf1();
    void mf3();
    void mf4();
};
```

**이 코드에서 기본 클래스에 있는 함수들 중에 mf1 및 mf3 이라는 이름이 붙은 것은 모두 파생 클래스에 들어 있는 mf1 및 mf3에 의해 가려지고 만다.** 이름 탐색의 시점에서 보면, 어처구니없게도 Base::mf1과 Base::mf3는 Derived가 상속한 것이 아니게 된다.

```cpp
Derived d;
int x;

d.mf1(); // Derived::mf1() 호출
d.mf1(x); // 에러 Derived::mf1이 Base::mf1을 가린다.

d.mf2(); // Base::mf2() 호출

d.mf3(); // Derived::mf3() 호출
d.mf3(x); // 에러 Derived::mf3이 Base::mf3을 가린다.
```

이런 규칙이 존재하는 이유는 어떤 라이브러리나 응용프로그램 프레임워크를 이용해 파생 클래스를 하나 만들 때, 멀리 떨어져 있는 기본 클래스로부터 오버로드 버전을 상속시키는 경우를 막겠다는 것이다.

이는 일종의 실수를 막겠다는 것이다. 또한 오버로드 함수를 상속받지 않겠다는 것도 엄연한 is-a 관계의 위반이다. 따라서 가려진 이름은 `using`선언문을 통해서 끄집어낼 수 있다.

```cpp
class Derived : public Base {
public:
    using Base::mf1;
    using Base::mf3;
    virtual void mf1();
    void mf3();
    void mf4();
};
```

만약 public상속이 아닌 private상속이라면 using선언으로는 해결할 수 없다. 그 이유는 using선언으로 내리면 그 이름에 해당하는 것들이 모두 파생 클래스로 내려가 버리기 때문이다. 따라서 간단한 전달 함수 기법(forwarding function)을 사용한다.

```cpp
class Derived : private Base {
public:
    virtual void mf1() {
        Base::mf1();
    }
    void mf3() {
        Base::mf3();
    }
    void mf4();
};
```

*간단한 전달 함수는 인라인 함수가 된다.*

### 정리

- 파생 클래스의 이름은 기본 클래스의 이름을 가린다. public 상속에서는 이런 이름 가림 현상은 바람직하지 않다.
- 가려진 이름을 다시 볼 수 있게 하는 방법으로, using 선언 혹은 전달 함수를 쓸 수 있다.
