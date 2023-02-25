---
title: "Component Pattern"
excerpt: " "

categories:
  - Patterns
tags:
  - [Patterns, Dev]

toc: true
toc_sticky: true
 
date: 2023-02-07
last_modified_at: 2023-02-07
---

# Component Pattern..?  

사실 컴포넌트 패턴은 유니티 그 자체라 딱히 설명할 것이 없는데 컨테이너의 개념을 좀 더 정리해볼려고 한다.  

디 커플링 패턴으로 유니티에서 우리가 오브젝트에 컴포넌트를 붙이는 행동 **자체**가 컴포넌트 패턴이다.  

![이미지](../../../assets/images/Unity_img/Patterns/ComponetPatterns00.png)  

player를 제작한다고 했을 때 처음 제작한 경우에는 한가지 Player.cs라는 스크립트에 입력, 이동, 점프, 공격... 모든 내용을 넣게 된다.  

결국 클래스가 거대해지며 커플링을 걷잡을 수 없을 만큼 커지고 사이드 이펙트의 지뢰밭이 될 수 있다.  

따라서 유니티에서 제작할 때는 InputComponent, AttackComponent등 컴포넌트로 만들어서 오브젝트에 부착하는 방식을 사용할 수 있다.  

유니티 자체가 컴포넌트 패턴을 사용하기 때문에 사용에 용이하지만..! 앞서 말한 컨테이너 or 관리자의 개념이 들어가면 좀 더 유용하게 활용이 가능하다.  

일단 분리 자체는 필술적인 부분인 것 같고.. 서로 의존되는 기능들에 대한 통신문제가 발생하는데 다양한 방법이 있다.  

1. 컨테이너에서 상태를 변경하고 해당 상태의 기준으로 컴포넌트가 동작
2. 컴포넌트가 서로를 참조
3. 메세지로 통신  

1, 2번은 말 그대로 구현이 가능하기 때문에 오늘은 컨테이너의 입장으로 최대한 인터페이스를 활용하여 기능을 구현해본다.  

```cs
public class Player : MonoBehaviour
{
    private List<IMyComponent> Components;

    private void Start()
    {
        Components = new List<IMyComponent>();
        
        Components.Add(new InputComponet());
        Components.Add(new JumpComponet());
        Components.Add(new AttackComponet());
    }

    private void Update()
    {
        foreach (var Component in Components)
        {
            Component.UpdateMyComponent();
        }
    }

    public void SendMessageMyComponent()
    {
        foreach (var Component in Components)
        {
            Component.RecevieMessage();
        }
    }
}
```

```cs
public interface IMyComponent
{
    public void UpdateMyComponent();
    public void RecevieMessage();

}
```

생각하며 만들다 보니 정말 다양한 방식이 있을 것 같다는.. 델리게이트를 사용해서 compent측에서 참조를 걸어주는 방법이나 인터페이스를 좀 더 활용하면 이벤트 기반 프로그래밍과 같이 작성이 가능할 것 같다.  

각각의 장점, 단점이 있기 때문에 설계단계에서 잘 생각하고 만드는 게 좋을 것 같다.

