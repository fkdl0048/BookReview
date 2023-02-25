---
title: "Builder Pattern"
excerpt: " "

categories:
  - Patterns
tags:
  - [Patterns, Dev]

toc: true
toc_sticky: true
 
date: 2023-02-08
last_modified_at: 2023-02-08
---

# Builder Pattern..?  

빌더 패턴이란, 디자인 패턴중에서 생성 패턴에 속하는 패턴이다.  

[<mark style='background-color: #4F96F3'>위키 백과</mark>](https://ko.wikipedia.org/wiki/%EB%B9%8C%EB%8D%94_%ED%8C%A8%ED%84%B4)  

좋은 코드, 나쁜 코드를 읽던 중 나온 패턴으로 코드를 오용하기 어렵게 만들기 위한 수단으로 사용된다.  

가변 클래스는 오용하기 쉽기 때문에 불변의 객체를 만들기 위해 필드를 const, readonly로 둬서 생성시에만 초기화가 가능하도록 하는 것이 좋다.

빌더 패턴은 클래스를 두개로 나누어 빌더 클래스, 불변 클래스로 구분한다.  

값을 설정하기 위한 빌더 클래스는 가변적이지만 빌더 클래스를 호출한 클래스의 인스턴스는 불변적 속성을 가진다.  

**TextOptions.cs**
```cs
namespace BuilderPattern
{
    class TextOptions
    {
        private readonly string font;
        private readonly float? fontSize;

        public TextOptions(string font, float? fontSize)
        {
            this.font = font;
            this.fontSize = fontSize;
        }

        public string GetFont => font;
        public float? GetFontSize => fontSize;

    }
}
```

객체 생성시 생성자를 통해서 값을 초기화해야 생성이 가능하다.  

*지금은 값이 두개밖에 없지만 클래스의 값이 늘어난다면 생성자가 길어지고 가독성이 떨어지는 결과가 나온다.*

> private TextOptions textoptions = new TextOptions("test", null, null, ..., null);

빌더클래스를 사용하거나 팩토리메서드 패턴을 사용해서 줄일 수 있다.  

**TextOptionsBuilder.cs**
```cs
namespace BuilderPattern
{
    class TextOptionsBuilder
    {
        private readonly string font;
        private float? fontSize;

        public TextOptionsBuilder(string font)
        {
            this.font = font;
        }

        public TextOptionsBuilder SetFontSize(float fontSize)
        {
            this.fontSize = fontSize;
            return this;
        }

        public TextOptions build()
        {
            return new TextOptions(font, fontSize);
        }
    }
}
```

마찬가지로 불변적이어야 하는 값`font`는 빌더클래스에서도 생성자로 초기화를 받고 Setter메서드는 선택사항으로 주어진다.  

즉, 가변적인 경우를 열어주고 build를 통해 객체를 생성하는 것이다.  

```cs
namespace BuilderPattern
{
    class Program
    {
        static void Main(string[] args)
        {
            TextOptions textOptions = GetDefaultTextOptions();
            
            System.Console.WriteLine(textOptions.GetFont);
            System.Console.WriteLine(textOptions.GetFontSize);
        }

        static TextOptions GetDefaultTextOptions()
        {
            return new TextOptionsBuilder("ARIAL")
            .SetFontSize(12.0f)
            .bulid();
        }
    }
}
```

패턴들을 조금씩 배우다보니 대부분 한가지 클래스나 인터페이스를 사용하여 컨테이너를 만드는 과정을 가지는 것 같다.  

**더 나아가서**  

좀 더 c#의 기능들을 사용하여 더욱 오용하기 어렵게 만들 수 있다.  

불변 클래스의 생성자를 private으로 돌려서 빌더를 통해서만 생성 가능하도록하는 방법 등등..  

---  

*좋은 코드, 나쁜 코드의 내용을 참고하여 작성했습니다.*