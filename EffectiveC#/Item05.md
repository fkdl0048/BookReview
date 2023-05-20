# 아이템 5: 문화권별로 다른 문자열을 생성하려면 FormattableString을 사용하라

앞 `아이템 4`에서 보간 문자열 기능을 사용해서 변수와 서식을 결합하는 기능을 알아봤다.

하지만 여러 문화권과 다양한 언어를 다뤄야 하는 경우는 세부적인 제어가 필요하다.

FormattableString은 문자열 보간으로 생성된 반환값이 될 수도 있다는 것이다.

```cs
string first = $"It's the {DateTime.Now.Day} of the {DateTime.Now.Month} month";
```

```cs
FormattableString second = $"It's the {DateTime.Now.Day} of the {DateTime.Now.Month} month";
```

```cs
var third = $"It's the {DateTime.Now.Day} of the {DateTime.Now.Month} month";
```

만약 var형식으로 선언하면 이 변수는 string 객체가 될 수 있겠지만 FormattableString을 상속한 타입의 객체가 될 수도 있다.

실제로 이 코드를 컴파일하면 컴파일러는 컴파일타임에 몇 가지 조건을 고려하여 서로 다른 코드를 생성한다.

만약 