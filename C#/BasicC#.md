23.03.17

# __C# 문법__

<BR><bR>

## __목차__

- [new 한정자](#new-한정자)

    - [override와 new 비교](#override와-new)

    - [사용법](#사용법)

    - [간단한 사용 예시 1](#간단한-사용-예시-1)

    - [new 사용 예시 2](#new-한정자-사용법-예시1)

    - [new 사용 예시 2](#new-한정자-사용법-예시1)
<bR><bR>

## __new 한정자__

 __선언 한정자__ 로 사용되는 new 키워드는 기본 클래스에서 __상속된 멤버를 숨깁니다.__ 즉, __다형성을 가지지 않습니다.__

같은 이름, [시그니처](#메서드-시그니처)를 가진 부모 클래스의 멤버와 파생클래스의 멤버에 대해 

파생클래스 멤버가 기본 클래스에서 상속된 멤버의 숨김을 인식한다고 __명시하는 역할을 합니다.__

new 한정자를 쓰지 않아도 결과는 달라지지 않습니다. 가독성을 위한 명시적 역할이 큽니다.<br><Br>


* 그 외 역할

    - 형식의 인스턴스를 만듭니다.

    - 제네릭 형식 제약 조건으로 만듭니다.
<br><Br>

## __override와 new__

두 한정자 모두 파생 클래스의 메서드가 기본 클래스의 메서드와 동일한 이름을 사용할 수 있게 해줍니다.

__new 한정자__ 는 동일한 이름의 새 멤버를 만들고 원래 멤버를 숨기는 반면,

__override__ 한정자는 상속된 멤버에 대한 구현을 확장합니다.

override 와 new 를 한 멤버에 모두 사용하면 오류가 발생합니다. __두 한정자는 함께 사용할 수 없습니다.__ <br><Br>


### __사용법__

1. 동일한 멤버 이름을 사용하여 파생된 클래스에 해당 멤버를 선언합니다.

2. new 키워드를 사용하여 이를 한정합니다.<br><Br>

    _new 키워드는 public 앞이나 뒤에 모두 사용 가능합니다._

<br>

### __간단한 사용 예시 1__

```c#
public class BaseC
{
    public int x;
    // BaseC.Invoke
    public void Invoke(){}
}

public class DerivedC:BaseC
{
    // new 한정자가 쓰인 DerivedC.Invoke
    new public void Invoke(){}
}
```
위 예제에서 BasC.Invoke 는 DerivedC.Invoke 에 의해 숨겨집니다!<br><Br>


### __new 한정자 사용법 예시1__

_기본 클래스의 숨겨진 멤버에 엑세스하는 방법_

```c#
public class BaseC
{
    public static int x = 100;
    public static int y = 30;
}

public class DrivedC:BaseC
{
    new public static int x = 77;

    static void Main()
    {
        // 77
        Console.WriteLine(x);
        // 100
        Console.WriteLine(BaseC.x);
        // 30
        Console.WriteLine(y);
    }
}
```
<Br>

### __new 한정자 사용법 예시2__

_new 한정자를 사용하여 경고 메시지 제거하는 방법_

_숨겨진 멤버에 엑세스 하는 방법_

```c#
public class BaseC
{
    public class NestedC
    {
        public int x = 200;
        public int y;
    }
}

public class DerivedC:BaseC
{
    // new 한정자 사용 멤버
    new public class NestedC
    {
        public int x = 100;
        public int y;
        public int z;
    }

    static void Main()
    {
        NestedC n1 = new NestedC();

        BaseC.NestedC bn = new BaseC.NestedC();

        // NestedC 형식의 인스턴스인 n1, 
        //NestedC 는 new 키워드로 가려져 있기 때문에 
        //파생클래스인 DerivedC 클래스의 NestedC 의 멤버인 x 가 호출됩니다.
        // 100
        Console.WriteLine(n1.x);
        // bn 은 명시적으로 BaseC 의 NestedC 클래스의 형식으로 생성된 인스턴스이기 때문에 BaseC.NestedC.x 가 호출됩니다.
        // 200
        Console.WriteLine(bn.x);
    }
}
```
<br><Br>

### __new 한정자 사용법 예시3__

```c#
class Car
{
    public void DescribeCar()
    {
        Console.WriteLine("기본 클래스 DescribeCar");
        ShowDetails();
    }

    public virtual void ShowDetails()
    {
        Console.WriteLine("기본 클래스 ShowDetails");
    }
}

class InheritanceCar1: Car
{
    // new 한정자 사용
    public new void ShowDetails()
    {
        Console.WriteLine("상속 1 ShowDetails");
    }
}

class InheritanceCar2: Car
{
    // override 한정자 사용: 오버라이딩
    public override void ShowDetails()
    {
        Console.WriteLine("상속 2 ShowDetails");
    }
}
```

위 예시에는 InheritanceCar1 과 InheritanceCar2 라는 Car 클래스를 상속받은 파생 클래스 두 개가 존재합니다.
```c#
    InheritanceCar1 car1 = new InheritanceCar1();
    InheritanceCar2 car2 = new InheritanceCar2();

    car1.ShowDetails();
    car2.ShowDetails();

    // 결과
    // 상속 1 ShowDetails
    // 상속 2 ShowDetails
```

ShowDetails 메서드는 각각 new와 override로 파생클래스 안에서 재구현 되었습니다.

위 예시에서는 둘 다 파생클래스 안의 메서드가 실행되어 출력됩니다.

하지만 아래 예시에서는 다릅니다.

```c#
    Car car1 = new InheritanceCar1();
    Car car2 = new InheritanceCar2();

    car1.ShowDetails();
    car2.ShowDetails();

    // 결과
    // 기본 클래스 ShowDetails
    // 상속 2 ShowDetails
```

기본 클래스 타입으로 생성된 파생 클래스의 인스턴스를 생성했습니다.

따라서 부모 클래스에 있는 ShowDetail이 우선적으로 실행되는데,

오버라이딩 되지 않은 InheritanceCar1의 메서드는 부모 메서드가 실행되고

오버라이딩된 IngeritanceCar2 의 메서드는 파생 클래스의 메서드가 실행됩니다.

<br><Br>

[참고 문서 1](https://learn.microsoft.com/ko-kr/dotnet/csharp/programming-guide/classes-and-structs/knowing-when-to-use-override-and-new-keywords)


<bR><bR>

## __Closer 람다__

<br><Br>

## __가비지 컬렉터 버전__

<br><Br>

## __Abstract 클래스__

<BR><bR>

## __Interface, Struct 의 상속__

<br><Br>

## __*.meta 파일__

<br><Br>


----

## __관련 지식__
<br>

### __메서드 시그니처__

매서드의 이름과 매개변수 리스트를 말합니다.

i.e. 오버로딩의 조건 [원문](#new-한정자)
<br><Br>