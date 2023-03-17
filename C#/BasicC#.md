23.03.17

# __C# 문법__

<BR><bR>

## __목차__

- [new 한정자](#new-한정자)
<bR><bR>

## __new 한정자__

new 키워드는 보통 객체를 생성하고 생성자를 호출해주는 기능으로 많이 사용합니다.

하지만 __선언 한정자__ 로 사용되는 new 키워드는 기본 클래스에서 __상속된 멤버를 숨깁니다.__

즉, 상속을 사용한 이름 숨기기를 합니다. 따라서  파생 버전의 멤버로 기본 클래스 버전의 멤버를 대신하게 됩니다.

* 역할

    - 형식의 인스턴스를 만듭니다.

    - 제네릭 형식 제약 조건으로 만듭니다.

## __new 한정자 사용해야하는 경우__

- 

<br><Br>

### __사용법__

1. 동일한 멤버 이름을 사용하여 파생된 클래스에 해당 멤버를 선언합니다.

2. new 키워드를 사용하여 이를 한정합니다.<br><Br>

___예시 1___

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
위 예제에서 BasC.Invoke 는 DerivedC.Invoke 에 의해 숨겨집니다!

### __주의할 점__

override 와 new 를 한 멤버에 모두 사용하면 오류가 발생합니다. 두 한정자는 함께 사용할 수 없습니다.

__new 한정자__ 는 동일한 이름의 새 멤버를 만들고 원래 멤버를 숨기는 반면,

__override__ 한정자는 상속된 멤버에 대한 구현을 확장합니다.<br><Br>

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

<bR><bR>

## __Closer 람다__

<br><Br>

## __가비지 컬렉터 버전__

<br><Br>

## __Abstract 클래스__

<BR><bR>

## __Interface, Struct 의 상속__

<br><Br>