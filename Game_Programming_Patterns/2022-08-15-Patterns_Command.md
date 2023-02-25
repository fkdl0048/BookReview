---
title: "Command Pattern"
excerpt: " "

categories:
  - Patterns
tags:
  - [Patterns, Dev]

toc: true
toc_sticky: true
 
date: 2022-08-15
last_modified_at: 2022-08-15
---

# 명령 패턴  

`Command Patterns`이란, 메서드 호출을 실체화한 것이다.  

여기서 말한 실체화란, 실제하는 것으로 만든 다는 뜻으로 프로그래밍 분야에서는 객체를 뜻한다.  

따라서 위의 문장을 해석한다면 함수의 호출을 객체로 감쌌다는 의미가 된다.  

비슷한 예로 `콜백`, `일급 함수`, `함수 포인터`, `클로저`, `부분 적용 함수`등이 있다.  

GoF책에서는 명령 패턴을 콜백함수를 객체지향적으로 표현한 것이라고 정의한다.  

## 예제 1번  

> 리플렉션 시스템은 런타임중에 자료형을 가져와서 다룰 수 있게 한다.(실체화된 자료형 체계)  

게임에는 키보드나 마우스등 유저의 입력을 읽는 코드가 존재한다.  

```cs
private void Update()
{
    if (Input.GetKeyDown(KeyCode.DownArrow))
        Debug.Log("아래");
    else if (Input.GetKeyDown(KeyCode.LeftArrow))
        Debug.Log("왼쪽");
    else if (Input.GetKeyDown(KeyCode.RightArrow))
        Debug.Log("오른쪽");
    else if (Input.GetKeyDown(KeyCode.UpArrow))
        Debug.Log("위쪽");
}
```

위 처럼 동작하는 코드가 있다고 할 때 명령 패턴은 위의 코드 실행부분들을 객체화하여 관리하는 것이다.  

```cs
// Command.cs
public abstract class Command
{
    public abstract void excute();
}

public class CommandRightMove : Command
{
    public override void excute()
    {
        RightMove();
    }

    private void RightMove()
    {
        Debug.Log("오른쪽");
    }
}

public class CommandLefttMove : Command
{
    public override void excute()
    {
        LefttMove();
    }

    private void LefttMove()
    {
        Debug.Log("왼쪽");
    }
}

public class CommandDownMove : Command
{
    public override void excute()
    {
        DownMove();
    }

    private void DownMove()
    {
        Debug.Log("아래");
    }
}

public class CommandUpMove : Command
{
    public override void excute()
    {
        UpMove();
    }

    private void UpMove()
    {
        Debug.Log("위");
    }
}
```

이렇게 Command 인터페이스를 만들고 상속받는 각 명령을 클래스로 만들어 두면 된다.  

이렇게 관리하게 되면 런타임중에도 해당 키에 맞는 명령을 배치할 수 있고 최상위 부모로 `Invoker`로 전체 실행등 다양한 동작이 가능하다.  

```cs
// Player.cs
public class Player : MonoBehaviour
{
    private Command _leftCommand;
    private Command _rightCommand;
    private Command _downCommand;
    private Command _upCommand;

    private void Start()
    {
        _leftCommand = new CommandLefttMove();
        _rightCommand = new CommandRightMove();
        _upCommand = new CommandUpMove();
        _downCommand = new CommandDownMove();
    }

    private void Update()
    {
        if (Input.GetKeyDown(KeyCode.DownArrow))
            _downCommand.excute();
        else if (Input.GetKeyDown(KeyCode.LeftArrow))
            _leftCommand.excute();
        else if (Input.GetKeyDown(KeyCode.RightArrow))
            _rightCommand.excute();
        else if (Input.GetKeyDown(KeyCode.UpArrow))
            _upCommand.excute();
    }
}
```

처음보면 뭐가 다른지 이해가 안될 수 있지만 직접함수를 호출하는 방식 대신에 한번 우회하는 방식으로 동작한다.  

이게 처음 방식과 다른 점은 런타임중에 키에 대한 명령을 아주 쉽게 변경이 가능하다.  

```cs
// Invoker.cs
public class Invoker
{
    private List<Command> _commands = new List<Command>();

    public void AddCommand(Command command)
    {
        _commands.Add(command);
    }
    
    public void RunCommand()
    {
        foreach (var VARIABLE in _commands)
        {
            VARIABLE.excute();
        }
    }
}
```

`Invoker`클래스를 새로 선언해서 객체화 된 메소드를 관리하여 사용이 가능하다.  

```cs
// Player.cs
public class Player : MonoBehaviour
{
    private Command _leftCommand;
    private Command _rightCommand;
    private Command _downCommand;
    private Command _upCommand;
    
    private void Start()
    {
        _leftCommand = new CommandLefttMove();
        _rightCommand = new CommandRightMove();
        _upCommand = new CommandUpMove();
        _downCommand = new CommandDownMove();

        Invoker invoker = new Invoker();
        
        invoker.AddCommand(_leftCommand);
        invoker.AddCommand(_rightCommand);
        invoker.AddCommand(_upCommand);
        invoker.AddCommand(_downCommand);
        
        invoker.RunCommand();
    }
```  

## 예제 2번  

예제 1번은 어쩔 수 없이 커플링이 발생하는 요소가 있다.  

따라서 객체를 함수에서 직접 찾지 말고 외부에서 전달해주는 방법으로 분리하게 되면 디커플링의 형태를 유지할 수 있다.  

```cs
// Command.cs
public abstract class Command
{
    public abstract void excute(GameActor actor);
}

public class CommandRightMove : Command
{
    public override void excute(GameActor actor)
    {
        actor.RightMove();
    }
}

public class CommandLefttMove : Command
{
    public override void excute(GameActor actor)
    {
        actor.LefttMove();
    }
}

public class CommandDownMove : Command
{
    public override void excute(GameActor actor)
    {
        actor.DownMove();
    }
}

public class CommandUpMove : Command
{
    public override void excute(GameActor actor)
    {
        actor.UpMove();
    }
}
```

> 외부에서 actor를 넘겨주고 해당 메서드를 실행

```cs
//GameActor.cs
public class GameActor
{
    public void RightMove()
    {
        Debug.Log("오른쪽");
    }
    public void LefttMove()
    {
        Debug.Log("왼쪽");
    }
    public void DownMove()
    {
        Debug.Log("아래");
    }
    public void UpMove()
    {
        Debug.Log("위");
    }
}
```

```cs
//Player.cs
public class Player : MonoBehaviour
{
    private GameActor actor;
    
    private Command _leftCommand;
    private Command _rightCommand;
    private Command _downCommand;
    private Command _upCommand;
    
    private void Start()
    {
        _leftCommand = new CommandLefttMove();
        _rightCommand = new CommandRightMove();
        _upCommand = new CommandUpMove();
        _downCommand = new CommandDownMove();

        actor = new GameActor();
    }

    private void Update()
    {
        if (Input.GetKeyDown(KeyCode.DownArrow))
            _downCommand.excute(actor);
        else if (Input.GetKeyDown(KeyCode.LeftArrow))
            _leftCommand.excute(actor);
        else if (Input.GetKeyDown(KeyCode.RightArrow))
            _rightCommand.excute(actor);
        else if (Input.GetKeyDown(KeyCode.UpArrow))
            _upCommand.excute(actor);
    }
}
```

actor는 게임 월드를 돌아다니는 캐릭터를 대표하는 '게임 객체' 클래스이다.  

즉, 해당 클래스를 상속받은 클래스의 객체또한 해당 메서드를 명령패턴으로 런타임에 연결할 수 있다.  

원하는 오브젝트의 메서드를 실행할 수 있는 것  

이 패턴을 공부하면서 생각이 든 방식은 숨바꼭질 같은 게임을 할때 물건으로 변신기능의 경우 해당 오브젝트로 제어권이 넘어가야하기 때문에 이러한 방식을 사용할 것 같다.(런타임중에 변경)  

Command자체를 최상위 객체로 만들어서 커플링을 없애는 방식인 것  

책에서는 명령자체를 큐나 스트림으로 관리하는 것을 추천한다.  

## 예제 3번  

명령패턴이 많이 사용된다는 실행취소 기능이다.  

앞선 명령패턴을 이해했다면 정말 쉽게 이 예제가 이해가 된다..!  

*메서드 자체를 객체화해야지 뒤로가기가 쉬워짐*

```cs
//GameActor.cs
public class GameActor
{
    private Vector3 _pos = Vector3.zero;
    public Transform Transform { get; set; }

    public GameActor(Transform transform)
    {
        Transform = transform;
    }

    public void MoveTo(Vector3 pos)
    {
        Transform.position = pos;
    }
}
```

> 정말 간단한 게임 객체 현재는 움직임을 설정할 수 있는 기능이 있다.

```cs
//Command.cs
public abstract class Command
{
    public virtual void Execute(GameActor actor, Vector3 newPos) { }
    public virtual void Undo(GameActor actor) { }
}

public class MoveUnitCommand : Command
{
    private Vector3 _prevPos;

    public override void Execute(GameActor actor, Vector3 newPos)
    {
        _prevPos = actor.Transform.position;
        actor.MoveTo(newPos);
    }
    public override void Undo(GameActor actor)
    {
        actor.MoveTo(_prevPos);
    }
}
```

> 전 Command는 추상화를 통해 격리, 분리하였지만 이번에는 유닛과 위치값을 받아서 바인딩하였다. 
> 따라서 언제든지 사용가능한 형태가 아닌 게임에서의 구체적인 이동의 형태를 가진 메서드  

```cs
public class Player : MonoBehaviour
{
    private GameActor _actor;
    private Stack<Command> _stack = new Stack<Command>();
    private void Start()
    {
        _actor = new GameActor(gameObject.transform);
    }

    Command GetCommand(ref bool isUndo)
    {
        if (Input.GetKeyDown(KeyCode.RightArrow))
        {
            Command command = new MoveUnitCommand();
            _stack.Push(command);
            return command;
        }
        if (Input.GetKeyDown(KeyCode.LeftArrow))
        {
            isUndo = true;
            return _stack.Count > 0 ? _stack.Pop() : null;
        }

        return null;
    }

    private void Update()
    {
        bool isUndo = false;
        Command command = GetCommand(ref isUndo);
        if (command != null)
        {
            if(!isUndo)
                command.Execute(_actor, new Vector3(Random.Range(0f,1f),Random.Range(0f,1f) ,Random.Range(0f,1f))); 
            else
                command.Undo(_actor);
        }
    }
}
```

> 플레이어에서 사용된 명령어를 담는 스택을 만듬 Command메서드 객체가 들어간다.  
> 오른쪽화살표를 누르면 움직임 메서드를 객체로 생성하고 push한다.
> 왼쪽 화살표를 누를 경우 스택의 양을 체크하고 pop한다.  
> 해당 명령을 수행하면 메서드 자체를 왔다갔다하며 수행이 가능하다.  

이번 명령패턴을 공부하며 느낀점인데 정말 범용성이 엄청나다는 것  

명령패턴이 생각보다 매우 유용하게 사용이 가능할 것 같다.  

---  

이 글은 `게임 프로그래밍 패턴(로버트 나이스트롬 지음/ 박일 옮김)`을 참고 하였습니다.