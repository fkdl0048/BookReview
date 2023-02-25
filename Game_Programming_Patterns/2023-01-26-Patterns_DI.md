---
title: "Dependency Injection"
excerpt: " "

categories:
  - Patterns
tags:
  - [Patterns, Dev]

toc: true
toc_sticky: true
 
date: 2023-01-26
last_modified_at: 2023-01-26
---

# Dependency Injection..?  

> 소프트웨어 디자인 패턴 중 기타 패턴에 속함

DI라고 불리는 의존성 주입이란, 의존 관계를 주입하는 객체가 의존하는 객체를 외부에서 선언하고 이를 주입받아 사용하는 것이다.  

객체간의 커플링, 의존관계를 줄이고 확정성을 가져간다.  

알게 모르게 사용했을 수 있는 패턴으로 인터페이스나 추상클래스를 활용하여 디커플링을 위한 코드를 작성하거나 봤다면 DI의 일종이라고 할 수 있다.  

모든 패턴이 그렇듯 단점도 존재한다.  

클래스나 인터페이스의 수가 늘어나 복잡성이 증가하고, 추적이 어렵다.  

## 의존성..?  

여기서 말하는 의존성은 클래스A가 클래스B를 사용한다면 클래스 A는 B에 의존성을 가지게 되고 이를 의미한다.  

```cs
class A
{
  B b = new B();
}

class B
{

}
```

## 의존성 주입  

그렇다면 의존성 주입은 말 그대로 클래스A에서 B를 만들어서 사용하느 것이 아닌 외부에서 B클래스의 인스턴스를 클래스A에게 주입하는 것을 의미한다.  

만약 서비스가 늘어나게 되면 즉, 클래스 C와 클래스 D가 생성되어 서비스가 추가된다면 아래와 같이 종속성때문에 종속되는 클래스들 또한 변경되어야 한다.  

```cs
public class B{}
public class C{}
public class D{}

public class A
{
    public B b {get; set;}
    public C c {get; set;}
    public D d {get; set;}
}
```

이제 의존성 자체를 외부에서 주입을 하기 위한 준비를 한다.  

```cs
public interface ITest {}

public class B : ITest{} 
public class C : ITest{}
public class D : ITest{}

public class A
{
    public ITest Connector {get; set;}
}
```

위 처럼 만들어지면 결합도 낮아지고 원하는 클래스를 넣어서 적절하게 사용가능하다.  

보자마다 그냥 인터페이스를 활용한 것 아닌가..? 라는 생각이 들지만 그게 맞다.  

다양한 행위패턴에서 등장하고 동적바인딩을 활용하거나 위 처럼 의존성자체를 직접 가지는 것이 아닌 외부의 결정에 일임하는 것  

의존성을 주입하는 방법에는 다양한 방법이 있는데 한번 알아보자아..!

### 1. 생성자 의존성 주입  

가장 기본적으로 많이 사용하는 방법으로 오버로딩을 통해 좀 더 다형성을 가질 수 있다.  

```cs
namespace DepenencyInjection
{
    public interface IFunction
    {
        void Function();
    }

    public class AFunction : IFunction
    {
        public void Function()
        {
            System.Console.WriteLine("새로운 A기술을 도입합니다.");
        }
    }

    public class BFunction : IFunction
    {
        public void Function()
        {
            System.Console.WriteLine("새로운 B기술을 도입합니다.");
        }
    }

    public class MyService
    {
        public IFunction Func {get; set;}

        public MyService(IFunction function)
        {
            Func = function;
        }

        public void FunctionAction()
        {
            Func.Function();
        }
    }

    class Program
    {
    
        static void Main(string[] args)
        {
            MyService service = new MyService(new AFunction());

            service.FunctionAction();

            service = new MyService(new BFunction());

            service.FunctionAction();
        }
    }
}
```

*상속이나 추상클래스는 클래스 자체의 기능을 이어받는 느낌이라면 인터페이스는 좀 더 기능적인 부분이 핵심이다.*  

이외에도 좀 더 활용하여 set함수로 만들어 변경하거나, 프로퍼티를 활용하여 주입할 수 있다.  

마찬가지로 이 패턴의 핵심은 의존성 자체를 느슨하게 가져가는 것이다.

---  

참고  

https://ko.wikipedia.org/wiki/%EC%9D%98%EC%A1%B4%EC%84%B1_%EC%A3%BC%EC%9E%85