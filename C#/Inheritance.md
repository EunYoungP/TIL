23.03.20

# __상속 Inheritance : C# 문법__

[마이크로소프트 공식 문서](https://learn.microsoft.com/ko-kr/dotnet/csharp/fundamentals/tutorials/inheritance)

---

<BR><bR>

## __목차__

- [상속](#상속)

- [private 멤버 상속](#pirvate-멤버-상속)

- [public 멤버 상속](#public-멤버-상속)

- [상속의 적용](#상속의-적용)

- [암시적 상속](#암시적-상속)

- [is a](#클래스-상속-이다is-a-관계)

- [기본-파생 클래스 디자인](#기본-클래스-파생-클래스-디자인)

- [추상-파생 클래스 디자인](#추상-클래스-파생-클래스-디자인)
<br><Br>

## __상속__

부모 클래스(기본 클래스)의 동작을 다시 사용(상속), 확장 또는 수정하는 자식 클래스(파생 클래스)를 정의할 수 있습니다.

C# 및 .NET은 __단일 상속__ 만 지원합니다. 전이적으로 상속 계층을 정의할 수는 있습니다. <BR><bR>

## __상속 못하는 멤버__

- 정적 생성자

- 인스턴스 생성자

- 종료자
<BR><bR>

## __pirvate 멤버 상속__

상속에서 private 멤버는 기본 클래스에 중첩된 파생 클래스에서만 표시됩니다.
```c#
public class A  // 기본 클래스
{
    private a;  // private 멤버
    public class B : A  // 중첩된 파생 클래스
    {
        public int GetValue()
        {
            return a;   // 기본 클래스의 private 멤버 사용
        }
    }
}
```
<br><Br>

## __public 멤버 상속__

Public 멤버는 파생 클래스에서 정의된 것처럼 호출할 수 있습니다.

```c#
public class A
{
    public int a = 100;
    public void Method1()
    {

    }
}

public class B : A
{
}

public class Example
{
    public static void Main()
    {
        B b = new();
        b.Method1();    // 기본 클래스의 public 멤버 사용
    }
}
```

기본 클래스의 메서드를 파생 클래스에서 재정의 하고싶은 경우, 기본 클래스의 메서드에 virtual, override 키워드를 사용하여 오버라이딩 합니다.

__abstract__ 키워드로 표현된 기본 클래스의 메서드는 반드시 __파생 클래스__ 에서 재정의해야 합니다.<BR><bR>


## __상속의 적용__

상속은 클래스, 인터페이스에만 적용됩니다.

구조체(struct), 대리자(delegate), 열거형(enum) 은 상속을 지원하지 않습니다.<br>

클래스, 구조체는 하나 이상의 인터페이스를 구현할 수 있습니다.

```C#
public abstract class A
{
    public abstract void Method1(){}
}

iterface IB
{
    public 
}

// 구조체는 인터페이스를 상속받을 수 있습니다.
// 클래스는 상속받을 수 없습니다.
public struct C : IB
{
}

// 인터페이스 또한 인터페이스를 상속받을 수 있습니다.
interface ID : IB
{

}

// 클래스는 인터페이스, 클래스 모두 상속받을 수 있습니다.
public void Class E : A, IB
{

}
```

__구조체__ 인터페이스를 다중으로 상속 받아 구현할 수 있습니다.

구조체는 값형식으로 스택에 할당됩니다.

하지만 상속받은 인터페이스의 멤버를 사용하는 순간 참조형식인 인터페이스에 의해 박싱을 일으켜 힙 영역에 스택 영역에 있던 데이터를 복제하여 참조객체를 생성합니다.

그리고 그 주소를 인터페이스 포인터가 가지고 있게 됩니다.

따라서 상속받은 인터페이스의 멤버 메서드를 호출하면 힙에 있는 참조객체의 값은 증가하지만 스택에 있는 구조체 값은 그대로 변하지 않는 오류가 발생할 수 있습니다.<BR><bR>

__인터페이스__ 는 인터페이스를 다중으로 상속 받아 구현할 수 있습니다.

하지만 인터페이스는 추상 클래스이기 때문에 선언부에 구현을 할 수 없다는 특징을 가지고 있어 상속받은 인터페이스의 멤버를 구현하지 않고 그대로 가지게 됩니다.

파생 인터페이스를 상속받은 파생 클래스에서 구현하여 사용할 수 있습니다.
<bR><bR>

__클래스__ 는 클래스, 인터페이스 모두 상속받을 수 있습니다.

클래스는 다중 상속 받을 수 없으며, 인터페이스는 다중 상속받을 수 있습니다.<BR><bR>


## __암시적 상속__

.NET 프레임 워크에서 궁극적인 클래스인 Object에 의해서 아래처럼 빈 클래스를 생성해도 [리플렉션](#리플렉션)을 사용해 클래스의 멤버 목록을 가져오면 실제로 9개이 멤버가 있는 것으로 출력됩니다.

```c#
public class SimpleClass{}
```
1개는 __생성자__ 이며, 나머지 8개는 .NET 형식 시스템의 모든 클래스 및 인터페이스가 마지막에 __암시적으로 상속하는 형식인 Object의 멤버__ 입니다.
<Br>

_8개의 멤버_
- ToString (Method):  Public, Declared by System.Object
- Equals (Method):  Public, Declared by System.Object
- Equals (Method):  Public Static, Declared by System.Object
- ReferenceEquals (Method):  Public Static, Declared by System.Object
- GetHashCode (Method):  Public, Declared by System.Object
- GetType (Method):  Public, Declared by System.Object
- Finalize (Method):  Internal, Declared by System.Object
- MemberwiseClone (Method):  Internal, Declared by System.Object
- .ctor (Constructor):  Public, Declared by SimpleClass

위 메서드들은 Object 클래스에서 암시적으로 상속되므로 SimpleClass 클래스에서 사용할 수 있습니다.<Br><Br>

## __클래스 상속 "~이다(is a)" 관계__

상속은 기본 클래스와 하나 이상의 파생 클래스 간 "~이다" 관계를 나타내는 데 사용됩니다.

_인터페이스 구현은 상속과는 다른 관계인 "[~할 수 있다(can do)](#할-수-있다-can-do)" 를 나타냅니다._ 
<Br>

파생 클래스는 기본 클래스의 __한 종류__ 입니다.

(e.g. 출판물을 나타내는 Publication 클래가 있고, 그 파생 클래스인 Book, Magazine 등이 있다면 책, 잡지 등은 출판물의 한 종류를 나타냅니다.)
<br><Br>


## __기본 클래스, 파생 클래스 디자인__

- 디자인 시 고려 사항

    - ___기본 클래스에 포함할 멤버___

    해당 코드를 공유할 가능성이 높은 경우 기본 클래스에 멤버로 추가해야합니다.

    파생 클래스마다 거의 동일한 멤버 구현을 제공하면 중복된 코드를 유지해야하기 때문에 버그가 발생하기 쉽습니다.

    - ___멤버에서 메서드 구현 제공 여부___

    파생 클래스에서 구현을 제공해야 한다면 abstract 또는 virtual 키워드로 재정의 할 수 있도록 합니다.

    - ___추상 클래스 여부___
    
    기본 클래스의 인스턴스화가 타당한지 판단하여 결정합니다.

    인스턴화가 적합하지 않은 경우 추상 클래스로 만듭니다.

    - ___파생 클래스 여부___

    상속 계층에서 최종 클래스를 나타내려고 한다면 __sealed__ 키워드를 통해 클래스를 상속 받을 수 없게 합니다.
<br><Br>

## __추상 클래스, 파생 클래스 디자인__

대붑분의 경우 기본 클래스는 추상 클래스로 선언되어 파생 클래스에서 구현해야 하는 멤버를 정의하는 __템플릿__ 으로 정의됩니다.<br><Br>

___추상 클래스 상속 클래스 다이어그램___

<img src = "https://user-images.githubusercontent.com/80774412/226921015-ecc7f0f1-5bac-468d-91e7-b8b6d0ad11f2.png"></img>

__Shape__ 추상 클래스를 상속받은 Square, Rectangle, Circle 파생 클래스들은 

각각 Shpae 클래스의 abstract 멤버를 상속받아 각자 코드에서 구현하고있습니다.

그리고 이 경우 파생 클래스들을 Shape 형식으로 업캐스팅 하여 배열이나 리스트 등으로 하나로 묶을 수 있습니다.

```c#
Shape shapes[] = {new Square(10), new Rectangle(10,15), new Circle(12)};
List<Shape> shapes = new List<Shape>(){new Square(10), new Rectangle(10,15), new Circle(12)};
```
Shape 형식으로 선언된 리스트/배열에 각 파생 클래스의 생성자를 이용하여 값을 초기화 해주어 인스턴스화 했습니다.

이렇게 하면 abstract 클래스에 abstract 멤버들을 이용하여 파생 클래스들의 요소들을 한번에 관리할 수 있습니다.
---

## __관련 지식__ 
<Br>

### __리플렉션__

어떠한 형식의 메타데이터를 검사하여 해당 형식에 대한 정보를 가져올 수 있게 하는것입니다.[원문](#암시적-상속)<br><br>

### __~할 수 있다 can do__

클래스의 상속과는 다르게 인터페이스 구현은 ~할 수 있다(can do) 관계로 표현합니다.

인터페이스는 해당 인터페이스를 구현 형식에서 사용 가능하게 만드는 기능 일부를 정의합니다.

클래스의 상속은 메서드의 구현까지 모두 상속받지만, 인터페이스는 가상클래스로 인터페이스 속에서 구현된 메서드의 선언이 불가하기 때문에 is a 관계가 성립 되지 않습니다.[원문](#클래스-상속-이다is-a-관계)<br><Br>