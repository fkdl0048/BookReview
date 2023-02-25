---
title: "State Pattern"
excerpt: " "

categories:
  - Patterns
tags:
  - [Patterns, Dev]

toc: true
toc_sticky: true
 
date: 2023-02-19
last_modified_at: 2023-02-19
---

# State Pattern  

> 객체의 내부 상태에 따라 스스로 행동을 변경할 수 있게 허가하는 패턴으로, 이렇게 하면 객체는 마치 자신의 클래스를 바꾸는 것처럼 보입니다.  

상태패턴.. 진작에 한번은 정리를 해야지 해야지 했는데.. 이제야 하게 된다.  

사실 앞서 다룬 패턴들과 크게 다르지 않다.  

전략패턴과도 비슷하고 커맨드, 컴포넌트등과 비슷한 형태를 가지기 때문에 사용쓰임에 따라 구분되는 것 같다.  

그리고 게임을 제작하다 보면 자연스럽게 만나기 때문에 크게 어렵지 않지만 병렬, 계층등 활용도가 매우 높다.

각 상태를 클래스로 표현하고 해당 상태가 바뀜에 따라서 알아서 해당 상태를 진행한다.  

구조는 간단하게 State라는 인터페이스를 상태 자체로 보고 객체가 들고 상태에 맞게 전환되어 해당 상태를 진행한다.  

```cs
public interface State
{
    public void Excute();
}

public class Attack : State
{
    public void Excute()
    {
        System.Console.WriteLine("공격");
    }
}

public class Jump : State
{
    public void Excute()
    {
        System.Console.WriteLine("점프");
    }
}

public class Jeonglee
{
    private State state;

    public Jeonglee(State state)
    {
        this.state = state;
    }

    public void Update()
    {
        state.Excute();
    }
}
```

상태의 전환부분은 일부러 적지 않았다.  

생성자 단계에서 DI를 통한 값을 입력받아 state의 변화는 객체 내부에서만 발생한다.  

좀 더 나가아가서 state 

```cs
public abstract class State<T> where T : class
{
    public abstract void Enter(T entity);

    public abstract void Execute(T entity);

    public abstract void Exit(T entity);
}
```

```cs
public class StateMachine<T> where T : class
{
    private T ownerEntity;
    private State<T> currentState;
    public State<T> previousState;
    private State<T> globalState;
    
    public void SetUp(T owner, State<T> entryState)
    {
        ownerEntity = owner;
        currentState = null;
        previousState = null;
        globalState = null;

        ChangeState(entryState);
    }
    
    public void Execute()
    {
        if (globalState != null)
        {
            globalState.Execute(ownerEntity);
        }
        
        if (currentState != null)
        {
            currentState.Execute(ownerEntity);
        }
    }

    public void ChangeState(State<T> newState)
    {
        if (newState == null)   return;

        if (currentState != null)
        {
            previousState = currentState;

            currentState.Exit(ownerEntity);
        }

        currentState = newState;
        currentState.Enter(ownerEntity);
    }
    
    public void SetGlobalState(State<T> newState)
    {
        globalState = newState;
        
        globalState.Enter(ownerEntity);
    }

    public void RevertToPreviousState()
    {
        ChangeState(previousState);
    }
}
```

게임에서 유리하게 사용하기 위해 진입점 종료점을 추가하고 상태의 관리 다형성을 위해 제네릭을 사용할 수 있다.  

이렇게 StateMachine의 형태로도 제작이 가능..