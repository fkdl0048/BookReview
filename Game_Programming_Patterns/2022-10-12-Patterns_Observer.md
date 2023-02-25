---
title: "Observer Pattern"
excerpt: " "

categories:
  - Patterns
tags:
  - [Patterns, Dev]

toc: true
toc_sticky: true
 
date: 2022-10-12
last_modified_at: 2022-10-12
---

# 옵저버 패턴  

> 객체 사이에 일 대 다의 의존 관계를 정의해두어, 어떤 객체의 상태가 변할 때 그 객체에 의존성을 가진 다른 객체들이 그 변화를 통지 받고 자동으로 업데이트될 수 있게 만듭니다.

옵저버 패턴이란, 객체의 상태 변화를 관찰하는 관찰자들, 옵저버의 목록을 객체로 등록하여 상태 변화가 있을 때 마다 메서드를 통해 객체가 직접 옵저버에게 통지하는 디자인 패턴이다.  

[옵저버 패턴이란?](https://ko.wikipedia.org/wiki/%EC%98%B5%EC%84%9C%EB%B2%84_%ED%8C%A8%ED%84%**B4**)  

정리 하다 보니 이벤트 주도적 프로그래밍(이벤트 기반 프로그래밍)에서 사용되는 디자인 패턴이라는 걸 쉽게 알 수 있다.  

[이벤트 주도적 프로그래밍](https://fkdl0048.github.io/unity/unity_in_EventDrivenProgramming/)  

앞 서 포스팅한 이벤트 주도적 프로그래밍의 내용과 같이 이벤트 관리 매니저를 두고 다른 리스너를 등록 또는 삭제하여 이벤트가 발생할 때 마다 모든 리스너들에게 통지해준다.  

위 포스팅에서는 인터페이스를 활용하여 싱글톤으로 매니저를 관리하여 각 객체가 직접 리스너가 되어서 쉽게 전달받는 형태로 만들었다.  

조금 알아보면 구현 방법이 정말 다양하다.  

또한, **MVC패턴(Model-View-Controller)구조**는 정말 흔한 구조인데 기반이 되는 패턴이 관찰자 패턴이다.  

패턴이 잘 감이 오지 않는다면 모든 상호작용을 update에서 처리하게 되면 엄청난 검사와 가독성이 매우 떨어지게 되는데 특정 값이 변경될 때(HP가 변경될 때 UI가 변경)처럼 이벤트를 등록하고 다른 리스너에게도 통지해주는 형식이다.  

> C#에는 IObserver, IObservable 옵저버 패턴에 사용되는 인터페이스가 제공된다.

유니티에서는 Unity Event를 열어두고 거기에 필요한 기능 사운드, 이펙트 재생등을 열어두거나 이벤트 기반 프로그래밍의 기본이 된다.  

유니티는 애니메이션의 event, Unity Event이외에도 툴만 잘 살펴봐도 옵저버를 사용한 이벤트가 많다.  

C#은 delegate, Event, -> action, func등 다양한 기능자체를 지원한다.  

따라서 적잘하게 하나의 subject를 가지는 것 보다 하나의 변수처럼 관리하는게 효율이 더 좋다.


C#의 인터페이스를 활용한 구현  
```cs
namespace ObserverPattern
{
    public interface Observer
    {
        public void OnNotify();
    }

    public interface ISubject
    {
        void AddObserver(Observer observer);
        void RemoveObserver(Observer observer);
        void Notify();
    }

    public class SbscriberA : Observer
    {
        public void OnNotify()
        {
            System.Console.WriteLine("A입니다.");
        }
    }

    public class SbscriberB : Observer
    {
        public void OnNotify()
        {
            System.Console.WriteLine("B입니다.");
        }
    }

    public class Entertainer : ISubject
    {

        List<Observer> observers = new List<Observer>();

        public void AddObserver(Observer observer)
        {
            observers.Add(observer);    
        }

        public void RemoveObserver(Observer observer)
        {
            observers.Remove(observer);
        }

        public void Notify()
        {
            foreach (var observer in observers)
            {
                observer.OnNotify();
            }
        }
    }

    class Program
    {
        public static void Main(string[] args)
        {
            Entertainer entertainer = new Entertainer();

            entertainer.AddObserver(new SbscriberA());
            entertainer.AddObserver(new SbscriberB());
            
            Observer observer = new SbscriberA();
            entertainer.AddObserver(observer);

            entertainer.Notify();

            entertainer.RemoveObserver(observer);
            entertainer.Notify();
        }
    }
}
```

델리게이트를 활용한 예제

```cs
namespace ObserverPattern
{
    delegate void OnEvent();

    class DelegateEntertainer
    {
        public event OnEvent myEvent;
        public void AddObserver(OnEvent sbscriber)
        {
            myEvent += sbscriber;
        }

        public void RemoveObserver(OnEvent sbscriber)
        {
            myEvent -= sbscriber;
        }

        public void Notify()
        {
            myEvent();
        }
    }
}
```

물론 델리게이트를 클래스에 넣어서 사용이 가능하지만.. 안전성 문제로 외부에서 호출할 수 없는 이벤트를 사용한다.  

---  

이 글은 `게임 프로그래밍 패턴(로버트 나이스트롬 지음/ 박일 옮김)`을 참고 하였습니다.