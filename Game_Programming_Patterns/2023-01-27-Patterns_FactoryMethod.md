---
title: "Factory Method Pattern"
excerpt: " "

categories:
  - Patterns
tags:
  - [Patterns, Dev]

toc: true
toc_sticky: true
 
date: 2023-01-27
last_modified_at: 2023-01-27
---

# Factory method pattern..?  

팩토리 메서드 패턴은 부모클래스에 알려지지 않은 구체 클래스를 생성하는 패턴이다.  

간단하게 말해서 객체를 찍어내는 공장클래스를 의미한다.  

팩토리 메서드 패턴에 대해서 정말 다양한 말이 존재하는데 간단한 `팩토리 함수`부터 `팩토리 메서드 패턴`까지 다양한 구현방법과 장점 그리고 단점을 알아본다.  

## Factory  

```cs
namespace FactoryMethodPattern
{
    public abstract class Pizza
    {
        public abstract float GetPrice();

        public enum PizzaType
        {
            HamMushroom,
            Deluxe,
            Seafood
        }

        public static Pizza PizzaFactory(PizzaType pizzaType)
        {
            switch (pizzaType)
            {
                case PizzaType.HamMushroom:
                    return new HamMushroom();
                case PizzaType.Deluxe:
                    return new Deluxe();
                case PizzaType.Seafood:
                    return new Seafood();
            }

            throw new System.NotSupportedException("The Pizza Type" + pizzaType.ToString() + "is not recognized.");
        }
    }

    public class HamMushroom : Pizza
    {
        private float price = 1.2f;
        public override float GetPrice () => price;
    }

    public class Deluxe : Pizza
    {
        private float price = 1.4f;
        public override float GetPrice () => price;
    }

    public class Seafood : Pizza
    {
        private float price = 1.0f;
        public override float GetPrice () => price;
    }

    class Program
    {
        static void Main(string[] args)
        {
            Pizza pizza = Pizza.PizzaFactory(Pizza.PizzaType.Seafood);
            System.Console.WriteLine(pizza.GetPrice());
        }
    }
}
```

위키백과에 나오는 C#의 예제이다.  

단번에 이해가 되고 직관적인 예제라 따로 수정하지 않고 그대로 가져왔다.  

몇가지 주의깊게 봐야하는 지점이 존재한다.  

앞서 포스팅한 DI부분 처럼 클래스 내부에서 해당 값을 가지게 되면 의존관계가 형성되게 되는데..  

해당 클래스의 생성자, 전처리가 변경되게 된다면 의존하는 **모든 Client의 코드를 변경**해야 하는 문제가 생긴다.  

하지만 위 처럼 팩토리함수로 한번 매핑을 하게 되면 클라이언트는 하나의 팩토리를 가리키고 해당 팩토리만 수정하게 되면 모든 클라이언트의 동작에 문제없이 대응할 수 있다.  

하지만 새로운 타입이 생길 때 마다 팩토리함수를 열어서 수정해야 함으로 팩토리가 아닌 팩토리 메서드 패턴을 알아야 한다.  

1. throw 처리 부분  

지금은 따로 try catch로 호출하는 쪽에서 감싸지 않았지만 따로 감싸준다면 예외처리가 깔끔하게 해결된다.  

즉 어느정도 enum의 미래상황에 대해서 대응이 가능하다는 것이다.  

```cs
static void Main(string[] args)
{
    try
    {
        Pizza pizza = Pizza.PizzaFactory(Pizza.PizzaType.Pepperoni); // 페페로니라는 새로운 피자 추가
        System.Console.WriteLine(pizza.GetPrice());
    }
    catch (System.Exception)
    {
        System.Console.WriteLine("예외처리..");
    }
}
```

throw가 없다면 프로그램은 크래쉬가 나서 종료될 것이고 문제가 될 수 있다.  

그렇다면 default를 사용하면 해결되지 않을까?? 라는 의문이 들 수 있지만 사용하는 것이 더 위험한 방법이다.  

명시적이 아닌 암시적인 해결방법이기 때문에 이후에 추가되는 enum의 값들이 전부 오용될 수 있다.  

*null방어도 마찬가지로 오히려 숨기는 방법*  

2. 팩토리 클래스의 변경  

앞서 말한 새로운 타입이 생기게 되면 팩토리함수의 내부를 수정해야 하기 때문에 유지보수에 좋은 코드는 아니다.  

물론 간단한 개발이나 추가적인 형식이 없다면 위의 방법을 채택하겠지만 항상 프로젝트가 바뀔 수 있음을 인지해야한다.  

```cs
public abstract class Pizza
{
    public abstract float GetPrice();

    public enum PizzaType
    {
        HamMushroom,
        Deluxe,
        Seafood,
        Pepperoni
    }

    public static Pizza PizzaFactory(PizzaType pizzaType)
    {
        switch (pizzaType)
        {
            case PizzaType.HamMushroom:
                return new HamMushroom();
            case PizzaType.Deluxe:
                return new Deluxe();
            case PizzaType.Seafood:
                return new Seafood();
            case PizzaType.Pepperoni:
                return new Pepperoni();
        }

        throw new System.NotSupportedException("The Pizza Type" + pizzaType.ToString() + "is not recognized.");
    }
}
```

한가지 타입은 해결은 되겠지만 얼마나 많은 피자가 추가될지 모르기 때문,,  

### Factory Method Pattern  

위의 단점을 해결하기 위한 팩토리 메서드 패턴이다.  

*위에서 다룬 코드는 그냥 팩토리 정도로 생각되는 것 같다. 의견의 차이가 있겠지만 여기서 다루는 내용이 좀 더 패턴에는 적합*  

기존의 단점이라고 한다면 기존 코드의 변경이 발생한다는 것인데 이를 해결하기 위해선 팩토리 클래스 자체를 한번 더 매핑해줘야 한다.  

즉, 생성되는 유닛하나당 새로운 팩토리를 가지게 되는 것  

인터페이스나 추상클래스로 공장의 틀을 만들고 해당 공장들을 각각 유닛들을 생성하게 한다.  

이후에 인터페이스로 공장을 참조하여 다양하게 찍어내는 방법.. DI까지 사용하게 된다면 생성할 때의 의존관계또한 줄일 수 있다.  

```cs
namespace FactoryMethodPattern
{
    public abstract class Pizza
    {
        public abstract float GetPrice();
    }

    public class HamMushroom : Pizza
    {
        private float price = 1.2f;
        public override float GetPrice () => price;
    }

    public class Deluxe : Pizza
    {
        private float price = 1.4f;
        public override float GetPrice () => price;
    }

    public class Seafood : Pizza
    {
        private float price = 1.0f;
        public override float GetPrice () => price;
    }


    public abstract class PizzaFactory
    {
        public abstract Pizza MakingPizza();
    }

    public class HamMushroomPizzaFactory : PizzaFactory
    {
        public override Pizza MakingPizza()
        {
            return new HamMushroom();
        }
    }

    public class DeluxePizzaFactory : PizzaFactory
    {
        public override Pizza MakingPizza()
        {
            return new Deluxe();
        }
    }
    
    public class SeafoodPizzaFactory : PizzaFactory
    {
        public override Pizza MakingPizza()
        {
            return new Seafood();
        }
    } 

    class Program
    {
        static void Main(string[] args)
        {
            PizzaFactory pizzaFactory = new SeafoodPizzaFactory();
            Pizza pizza = pizzaFactory.MakingPizza();

            System.Console.WriteLine(pizza.GetPrice());
        }
    }
}
```

*예시이기 때문에 따로 파일 분리를 하지 않음*

피자하나마다 공장을 하나씩 가지게 되는데 이렇게 되면 기존 코드의 수정이 일어나지 않는다.  

모듈화를 챙기게 된 모습..  

이번엔 추상클래스가 아닌 인터페이스를 활용해서 DI(의존관계주입)까지 활용한 코드이다.

```cs
namespace FactoryMethodPattern
{
    public abstract class Pizza
    {
        public abstract float GetPrice();
    }

    public class HamMushroom : Pizza
    {
        private float price = 1.2f;
        public override float GetPrice () => price;
    }

    public class Deluxe : Pizza
    {
        private float price = 1.4f;
        public override float GetPrice () => price;
    }

    public class Seafood : Pizza
    {
        private float price = 1.0f;
        public override float GetPrice () => price;
    }


    public interface IPizza
    {
        Pizza MakingPizza();
    }

    public class PizzaFactory
    {
        public IPizza pizza {get; set;}

        public PizzaFactory(IPizza pizza)
        {
            this.pizza = pizza;
        }

        public Pizza MakingPizza()
        {
            return pizza.MakingPizza();
        }
    }

    public class HamMushroomPizzaFactory : IPizza
    {
        public Pizza MakingPizza()
        {
            return new HamMushroom();
        }
    }

    public class DeluxePizzaFactory : IPizza
    {
        public Pizza MakingPizza()
        {
            return new Deluxe();
        }
    }
    
    public class SeafoodPizzaFactory : IPizza
    {
        public Pizza MakingPizza()
        {
            return new Seafood();
        }
    } 

    class Program
    {
        static void Main(string[] args)
        {
            PizzaFactory pizzaFactory = new PizzaFactory(new HamMushroomPizzaFactory());
            Pizza pizza = pizzaFactory.MakingPizza();

            System.Console.WriteLine(pizza.GetPrice());
        }
    }
}
```

인터페이스까지 넣게되니 결합도 자체는 많이 낮아지지만 가독성이나 클래스 수는 많이 늘어날 수 밖에 없다.  

앞서 추상클래스는 각각 공장에 일임하는 모습이지만 인터페이스를 활용하여 의존성을 주입하면 일괄 공장에서 처리 후, 전으로 관리를 할 수 있게 된다.

--- 

참고  

https://ko.wikipedia.org/wiki/%ED%8C%A9%ED%86%A0%EB%A6%AC_%EB%A9%94%EC%84%9C%EB%93%9C_%ED%8C%A8%ED%84%B4