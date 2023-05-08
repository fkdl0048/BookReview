## 4. 클래스 해부하기

클래스를 무작정 처음부터 설계하면 안되는 이유는 어떤 클래스도 고립된 섬 같은게 아니기 때문이다.

객체가 인스턴스화되면 객체는 거의 항상 그 밖의 객체와 상호 작용을 한다.

객체는 다른 객체의 일부이거나 상속 계층의 일부일 수도 있다.

### 4.1. 클래스의 이름

클래스의 이름은 매우 중요하다.

다른 개발 서적(clean code, effective java)에서도 클래스의 이름은 매우 중요하다고 강조한다.

클래스가 하는 일과 이름이 더 큰 시스템 내에서 상호 작용하는 방법에 대한 정보를 제공하기 때문에 이름을 잘 지어야 한다.

```java
using System;

namespace ConsoleApplication1
{
    class TestPerson
    {
        public static void Main()
        {
            Cabbie joe = new Cabbie("Joe", "1234");

            Console.WriteLine(joe.Name);
            Console.ReadLine();
        }
 }
    
    public class Cabbie 
    {
 
     private string _Name;
     private Cab _Cab;

     public Cabbie() {

         _Name = null;
           _Cab = null;

        }

        public Cabbie(string name, string serialNumber) {

         _Name = name;
         _Cab = new Cab(serialNumber);

     }

        //Methods  
        public String Name
        {
            get { return _Name; }
            set { _Name = value; }
        }

    }

    public class Cab 
    {
 
     public Cab (string serialNumber) {

         SerialNumber = serialNumber;

     }
        
        //The property is public to get, but private to set
        public string SerialNumber { get; private set; }
        
    }

}
```

#### 4.1.1. 주석

사용된 주석은 구문과 관계없이 함수를 이해하는 일종의 알림판이다.

주석에 대한 생각은 `CleanCode`, `GoodCode, BadCode`와 크게 다르지 않아서 생략한다.

*잘짜여진 코드는 주석이 필요없다. 일종의 변명이다.*

### 4.2. 속성

객체에 대한 정보를 저장하는 속성은 객체의 상태를 나타낸다.

- 가능한 많은 데이터를 숨기기

모든 속성을 최소한으로 유지하는 설계를 가져가고 메서드 인터페이스를 통해 소통한다.

`MyCab`이라는 속성은 그 밖의 객체를 가리키기 위한 참조이다.

- 참조 전달

Cab객체가 그 밖의 객체에 의해 생성되었을 수 있다.  

따라서 참조는 Cabbie객체로 전달된다.

그러나 이 예를 위해 Cab은 Cabbie 객체 내에 만들어진다.  

**결과적으로 우리는 실제로 Cab객체의 속을 들여다볼 생각이 없는 것이다.**

`private Cab _Cab;` 이 시점에는 Cab객체에 대한 참조만 생성된다는 점에 집중하자.

이 정의문만으로 메모리가 할당되지 않는다.

*4byte의 레퍼런스 변수 메모리만 할당됨*

### 4.3. 생성자

```java
public Cabbie() {
         _Name = null;
           _Cab = null;
        }

        public Cabbie(string name, string serialNumber) {

         _Name = name;
         _Cab = new Cab(serialNumber);
     }
```

첫 번째 생성자는 Default 생성자이다.

이 생성자는 시스템에서 제공하는 Default 생성자가 아니고 기본 생성자라고 부르는 이유는 인수가 없는 생성자이기 때문이다.

사실상 **컴파일러의 기본 생성자를 가지고 오버라이딩**한 것이다.

*앞 장에서 말한 것 처럼 시스템의 기본 생성자에 의존하지 말 것*

다중 생성자를 이상적인 방법으로 여겨지지 않는 경우가 있다.

### 4.4. 접근자

대부분의 속성의 경우 private으로 정의된다.

그렇다면 객체와 상호작용하지 않고 격리된 객체를 만드는 것이 올바른 일인가?

아니다.  

때로는 격리된 객체가 유용할 수 있지만 객체들과 상호작용하기 위해선 접근자의 수준을 잘 결정해야 한다.

객체 속성에 접근해야 하는 경우가 종종 있지만 그런 일을 객체가 직접 수행할 필요는 없다.

만약 객체 A가 객체 B를 제어할 권한이 없는데도 객체 B의 속성을 검사하거나 변경하는 기능을 갖게 되기를 바라지 않는다.

여기에는 몇 가지 이유가 있는데 가장 중요한 이유는 데이터 무결성과 효율적인 디버깅으로 귀결된다.

이러한 측면에서 get, set함수와 프로퍼티가 잘 활용된다.

*이러한 무결성을 get,set함수에 잘 처리된 로직을 추가하면 더욱 단단해진다.*

실제로 각 객체에 대한 비정적 메서드의 실제 사본은 존재하지 않는다.

각 객체는 동일한 물리적 코드를 가리킨다.

그러나 개념적 수준에서 볼 때는 객체가 완전히 독립적이며, 고유한 속성과 방법을 갖는 것으로 생각할 수 있다.

### 4.5. 공개 인터페이스 메서드

생성자와 접근자 메서드는 모두 공개(public)으로 선언되어 있으니, 이 둘은 모두 공개 인터페이스의 일부다.

이것들은 클래스 생성에 특별히 중요하다.

공개 인터페이스는 매우 추상적인 경향이 있으며, 구현부는 아주 구상적이다.

### 4.6. 비공개 메서드

클래스의 메서드들을 그 밖의 클래스로부터 숨기는 편이 더 일반적이다.

이러한 비공개 메서드는 공개 인터페이스가 아닌 구현부의 일부로 사용된다.

중요한 점은 비공개 메서드는 구현부의 일부이며, 그 밖의 클래스에서는 접근할 수 없다는 것이 핵심이다.

### 4.7. 결론

클래스가 어떻게 구축되는지 이해하고 기본 개념을 설명했다.

#### 4.7.1. 느낀점

마찬가지로 기본적인 객체지향 내용이다.
