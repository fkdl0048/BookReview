---
title: "Flyweight Pattern"
excerpt: " "

categories:
  - Patterns
tags:
  - [Patterns, Dev]

toc: true
toc_sticky: true
 
date: 2022-08-23
last_modified_at: 2022-08-23
---

# 경량 패턴  

경량패턴의 핵심은 공유를 통한 최적화이다.  

오브젝트 풀같은 경우 객체 자체를 재활용에 가까운 최적화라면 경량 패턴은 객체가 공유하는 하나의 객체를 참조하여 공유하는 것이다.  

맵에 정말 많은 나무를 배치한다고 했을 때 유니티는 하나의 스프라이트 랜더러(나무 그림)를 공유하고 각각의 나무는 transform정도의 차이가 있다.  

이미 GPU 인스턴싱이라는 기능이 유니티에 구현이 되어 있다.  

[GPU 인스턴싱](https://docs.unity3d.com/kr/current/Manual/GPUInstancing.html)  

```cs
public class Tree
{
    private MeshRenderer mesh;
    private Texture bark;
    private Texture leaves;
    private Vector3 position;
    private double height;
    private double thickness;
    private Color barkTint;
    private Color leafTint;
}
```

기본적으로 맵에 배치되는 나무가 있다고 했을 때 간단하게 이 정도 클래스가 있다고 할 수 있다.  

위 에서 말한 것 처럼 이 나무 클래스 객체를 100만개를 배치한다고 했을 때는 잡아먹는 메모리가 그 만큼 커지게 된다.  

그렇다면 **숲**을 만들기 위해서 나무 객체가 공유할 수 있는 부분을 뽑아본다.  

```cs
class TreeModel
{
    private Mesh mash;
    private Texture bark;
    private Texture leaves;
}
```

나무의 Mesh, Texture는 변하지 않는다 즉, 모든 나무는 한가지 객체만을 공유하여 화면에 보여주면 된다.  

```cs
public class Tree
{
    private TreeModel model;
    
    private Vector3 position;
    private double height;
    private double thickness;
    private Color barkTint;
    private Color leafTint;
}
```

이제 위 `Tree`객체는 기본적으로 `TreeModel`을 참조하게 된다.  

---  

이 글은 `게임 프로그래밍 패턴(로버트 나이스트롬 지음/ 박일 옮김)`을 참고 하였습니다.