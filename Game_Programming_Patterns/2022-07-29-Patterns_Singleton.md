---
title: "Singleton Patterns"
excerpt: " "

categories:
  - Unity
tags:
  - [Unity, Dev]

toc: true
toc_sticky: true
 
date: 2022-07-29
last_modified_at: 2022-07-29
---

# 유니티 디자인 패턴 - 싱글톤  

디자인 패턴 또는 프로그래밍 패턴은 중복되는 코드를 효율적인 해결책이다.  

많은 사람들이 말하길 '효율적인 코드를 만들기위한 방법론'정도로 설명한다. 

게임에 적용되는 디자인 패턴의 종류도 매우 다양하지만 그 활용도가 매우 높아서 필수적으로 활용해야 한다고 본다.  

오늘은 그중에서도 말이 많은 **싱글톤 패턴**을 알아본다.  

싱글톤 패턴이란, 유니티에서만 한정되는 것이 아닌 하나의 프로그래밍 패턴으로 전역적으로 접근 가능한 상태의 인스턴스를 생성하여 임의의 클래스에서도 전역적으로 접근가능하게 만드는 것이다.  

전역적으로 접근이 가능하다면 게임전체적인 흐름을 관장할 수 있기 때문에 매니저의 형태로 가장 많이 사용되며 사용에 편리하다.  

> 그렇다면 필요한건 다 싱글톤으로 만들면 되는거 아닌가..?  

**아니다.** 사용은 매우 간편하지만 많은 데이터나 메서드를 넣다보면 코드 전체적인 흐름이 망가지기 마련이다.  

처음에는 이말을 잘 이해하지 못했지만 한번 경험해보니 이유를 이해하기 시작했다.  

직접 경험해보는 것이 가장 이해가 빠르기 때문에 무작정 안좋다기 보다 직접 실패를 경험해보며 사용방법을 익히는게 좋다고 생각한다.  

*게임을 제작할 때 무분별하게 사용하게 되면 이후에 덩치가 커질 경우 의존도와 전체적인 수정이 대대적으로 이루어져야 하기 때문에 조심하는 것이 좋다.*

++ 중요한 unity lifecycle을 망가트리는 주범이라고 많이 말한다.(안티패턴)  

## 유니티 싱글톤 패턴 사용법  

구조를 설명하기 앞서 위에서 설명한 것 처럼 싱글톤 패턴자체가 프로그래밍 패턴이기 때문에 MonoBehaviour를 상속받지 않고(하이어라키에 존재하지 않음) 사용도 가능하다.  

1. MonoBehaviour를 상속받아서 Hierarchy상에 존재하는 것(즉, 씬에 존재)
2. MonoBehaviour를 상속받지 않고 Hierarchy상에도 존재하지 않는 것

1번의 경우 유일객체로 보존을 보장할 수 없기 때문에(instantiate생성, 컴포넌트 부착 등..) static으로 유일특성을 예외처리 해줘야 한다.  

*싱글톤 패턴이라고 해서 코드가 다 똑같은게 아니고 살짝식 다른 점이 존재한다. (프로퍼티 선언, 삭제 방식 등등..)*

### singleton(1)

1번 방식으로 싱글톤패턴을 구현하고 테스트 해본다.  

```cs
using UnityEngine;

/// <summary>
/// MonoBehaviour를 상속받아서 Scene에 존재하는 싱글톤패턴
/// </summary>
public class Singleton : MonoBehaviour
{
    private static Singleton instance = null;

    public static Singleton Instance
    {
        get
        {
            if (null == instance)
            {
                return null;
            }
            return instance;
        }
    }

    private void Awake()
    {
        if (instance == null)
        {
            instance = this;
            
            DontDestroyOnLoad(gameObject);
        }
        else
        {
            Destroy(gameObject);
        }
    }

    public void TestSingleton()
    {
        Debug.Log("singleton Test Log");
    }
}
```

기본적인 싱글톤 패턴의 모습이다.  

1. 우선 static으로 선언해서 유일의 속성을 가지게 하고 해당 인스턴스만 존재할 수 있게 만든다. 
2. private으로 캡슐화를 미리 만들어 두고 프로퍼티로 get메서드만 구현한다(null예외)
3. Awake로 시작하자마자 자신을 할당하고 만약 다른 씬에 같은 싱글톤이 존재한다면 새로 생긴 싱글톤을 삭제한다.  

```cs
using UnityEngine;

public class Temp : MonoBehaviour
{
    private void Start()
    {
        Singleton.Instance.TestSingleton();
    }
}
```

씬에 빈 오브젝트에 부착하여 실행해보면 문제없이 돌아감을 알 수 있다.  

장점이 명확하게 존재하는 부분은 다른 코드를 단 한줄로 참조하여 다룰 수 있는 점은 정말 좋다ㅠㅠ..  

### singleton(2)

```cs
using UnityEngine;

/// <summary>
/// MonoBehaviour를 상속받지 않는 싱글톤
/// </summary>
public class SingletonType2
{
    private static SingletonType2 instance;

    public static SingletonType2 Instance
    {
        get
        {
            if (instance == null)
            {
                instance = new SingletonType2();
            }
            return instance;
        }
    }
    
    public void TestSingleton()
    {
        Debug.Log("singleton Test Log");
    }
}
```

확연히 짧아진 코드이다.  

이유는 씬 이동에 신경쓰지 않아도 되서 해당 예외처리가 빠진 것(유일이 보장됨)  

말 그대로 static 함수를 꺼내다 쓰는 동작과 같다.(Math, Mathf)  

두 함수의 차이는 크게 다르지 않기 때문에 사용방식에 따라서 취향것 고르면 될것 같다.  

## 더 나아가서  

좀 더 예외처리에 중점을 두고 만들게 되면 제네릭을 사용해서 만들 수 있다.

*동기화, 코드 일반화, 오브젝트 문제 등등..*  

```cs
using UnityEngine;

public class SingletonType3<T> : MonoBehaviour where T : MonoBehaviour
{
    private static bool m_ShuttingDown = false;
    private static object m_Lock = new object();
    private static T m_Instance;

    public static T Instance
    {
        get
        {
            if (m_ShuttingDown)
            {
                Debug.LogWarning($"{typeof(T)} return null");
                return null;
            }

            lock (m_Lock)
            {
                if (m_Instance == null)
                {
                    m_Instance = (T)FindObjectOfType(typeof(T));

                    if (m_Instance == null)
                    {
                        var singletonObject = new GameObject();
                        m_Instance = singletonObject.AddComponent<T>();
                        singletonObject.name = typeof(T).ToString() + "(singleton)";
                        
                        DontDestroyOnLoad(singletonObject);
                    }
                }
            }

            return m_Instance;
        }
    }

    private void OnDestroy()
    {
        m_ShuttingDown = true;
    }

    private void OnApplicationQuit()
    {
        m_ShuttingDown = true;
    }
}
```

많이 발전된 형태로 코루틴의 형식 자체를 제네릭으로 만들어 필요한 매니저마다 상속받아서 사용할 수 있다.  

2번과 마찬가지로 하이어라키에 추가하여 사용하는게 아닌 코드상으로 Gameobject를 추가하여 시작할 때 인스턴스화 한다.  

```cs
using UnityEngine;

public class EtcManager : SingletonType3<EtcManager>
{
    protected EtcManager() { }
    
    public void TestSingleton()
    {
        Debug.Log("singleton Test Log");
    }
}
```

자기 자신을 제네릭으로 넘겨서 생성하고 기본 상속자를 제외하면 코루틴의 예외처리부분은 부모에 있기 때문에 위처럼 짧게 만들어서 각 매니저로 분리하여 사용가능하다.  

공부하다 보니 이렇게 유동적으로 사용할 수 있는 코루틴 형식을 보니 신기하다 각 매니저마다 만들어서 사용하는 줄 알았지만 이렇게 일반화가 되니 직접 분리하여 명확하게 적재적소에 사용한다면?(덩치가 크지 않다면) 무난하게 사용해도 좋을 것 같다.  

++ 2023/02/19  

싱글톤 패턴을 피할 수 있는 방법..?  

싱글톤 패턴은 게임 개발자라면 무조건 접하는 패턴이다 보니 위해서 말한대로 마구마구 남용하거나 무조건 문제를 직면하게 된다.  

전역변수는 참 달콤하지만 위험한 초콜릿이다.  

처음 개발자들이 가장 사용하기 편하고 작은 프로젝트에서 위험성을 느끼지 못하기 때문에 남용하게 되는데  

한번쯤은 이런 문제를 만나보는 것이 좋다고 생각한다.  

1. 전역 변수는 코드를 이해하기 어렵게 만든다.  

개발은 필수적으로 남과 같이 하는 작업이다. 여기서 남이란 미래의 나도 포함이다.  

함수에서 버그가 발견되어 찾아야 할 때 함수가 전역상태를 건드리지 않는다면 해당 함수만 보면 된다.  

하지만 전역 변수를 사용하게 되면 전체 코드를 뜯어봐야 하는 상황이 올 수 있다.  

물론 의존성이 강하게 스파게티처럼 연결되어 있어도 마찬가지 이지만..  

전역변수를 한번 도입하게 되면 코드는 사이드 이펙트의 지뢰밭이 되게 된다. 

2. 전역 변수는 커플링을 조장한다.  

앞서 말한 지뢰밭처럼 전역변수가 들어가는 순간 커플링이 강하게 생기게 되며 사용되는 코드마다의 재귀적인 커플링까지 발생한다.  

3. 전역 변수는 멀티스레딩 같은 동시성 프로그래밍에 알맞지 않다.  

멀티스레딩이 필수가 된 시대인 만큼 전역 변수를 조심해야 한다.  

나중에 가면 교착상태, 경쟁 상태를 전혀 찾지 못하는 문제가 찾아온다.  
