23.03.17

# __C# 문법__


## __목차__

- [new 한정자](#new-한정자)

    - [override와 new 비교](#override와-new)

    - [사용법](#사용법)

    - [간단한 사용 예시 1](#간단한-사용-예시-1)

    - [new 사용 예시 2](#new-한정자-사용법-예시1)

    - [new 사용 예시 2](#new-한정자-사용법-예시1)

- [Closure](#closure)

    - [Closure 개념](#closure-개념)

    - [Closure는 어디에 사용하는가](#closure는-어디에-사용하는가)

    - [Closure 구현](#closure의-구현)

- [세대별 가비지 컬렉터](#세대별-가비지-컬렉터-알고리즘)

    - [개념](#세대별-가비지-컬렉터)

    - [세대별 객체 수명 관리](#객체의-수명-예측은-어떻게)

    - [가비지 컬렉션 줄이기](#가비지-컬렉션-줄이는-방법)
<br><Br>


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


[참고 문서 1](https://learn.microsoft.com/ko-kr/dotnet/csharp/programming-guide/classes-and-structs/knowing-when-to-use-override-and-new-keywords)


<bR><bR>

## __Closure__

C# 2.0부터 지원된 기능으로 __무명메서드Anonymous Method__ 와 __람다식Lambda Expression__ 으로 구현 가능합니다.

### __Closure 개념__

무명메서드나 람다식이 그것을 정의하고 있는 메서드의 로컬변수를 사용하고 있을 때,

그 무명메서드 혹은 람다식은 Closure라 부릅니다.

```c#
class MyClass1
{
    // 익명메서드를 감싸고있는 메서드
    public void Test()
    {
        int key = 10;   // Outer 로컬변수
        public Action<string> print = delegate(string msg)
        {
            string str = msg + key;
        }
        print("A");
    }
}
```

print 의 익명메소드 속에서 바깥쪽 함수의 변수(Outer 로컬변수)인 key를 사용합니다.

이런 경우 key의 값은 Test함수를 실행하고 벗어나는 순간 사라집니다.

key는 로컬변수로 그 메서드를 벗어나는 순간 스택에서 지워지기 때문입니다.

따라서 C# 컴파일러는 Closure을 만들어 특별 처리를 해줍니다.

Outer 로컬변수를 스택에 두지 않고 힙에 두고, 별도의 Nested Class를 생성하여 그 필드에 Outer 로컬 변수를 저장하고 람다식을 메서드에 저장합니다.

아래 코드는 위 코드를 C# 컴파일러가 실행하여 Nested로 실행되는 개념적 모습입니다.

```c#
Class MyClass1
{
    // Nested 클래스를 생성합니다.
    Class Nested
    {
        // Outer 로컬변수를 필드에 저장합니다.
        public int key;
        // 람다식을 메서드로 추가합니다.
        public void print(string msg)
        {
            string str = msg + key;
        }
    }

    // Outer 클래스에서 Nested 객체를 엑세스하고
    // 메서드를 호출합니다.
    public void Test()
    {
        Nested n = new Nested();

        n.key = 10;
        n.print(a);
    }
}
```
<Br><Br>

### __Closure는 어디에 사용하는가?__

기본적으로 Closure 는 [일급함수](#일급-함수)로서 전달할 수 있는 함수입니다.

함수를 이리 저리 전달하여 사용할 때,

그 함수가 처음 정의될 때의 데이터를 그대로 가지고 있을 필요가 있는 경우에 사용합니다.

LINQ 쿼리에서 이런 기능이 많이 사용됩니다.

```C#
class Class1
{
    private List<string> lists = new List<string>()
    {
        "A","B","ABC"
    };

    public IList<string> GetList(int maxLength) // Outer 메서드의 입력 파라미터
    {
        var limitedList = lists.Where(q => q.Length <= maxLegnth).ToList();

        return limitedList;
    }
}
```

maxLength는 Outer 메서드의 입력 파라미터로 Where 절 안의 람다식은 Closure가 되며,

C# 컴파일러는 C# Closoure에 맞는 Nested Class를 생성합니다.<br><Br>

## __Closure의 구현__

```c#
// Static 메서드 생성
class MyClass1
{
    public void Run()
    {
        Action act = () => Console.WriteLine("a");
    }
}

// Instance 메서드 생성
class MyClass2
{
    int a = 1;
    public void Run()
    {
        Action act = ()=>Console.WriteLine("A");
    }
}

// Nested 클래스 생성(Closure)
class MyClass3
{
    public void Run()
    {
        int a = 1;  // Outer 로컬 변수
        Action act = () => Console.WriteLine("a");
    }
}
```

__MyClass1__ 처럼 람다식에 외부 변수/필드를 전혀 사용하지 않으면 C# 컴파일러는 람다식에 대하여 static 메서드를 생성합니다.

__MyClass2__ 처럼 람다식에 객체 필드를 사용하는 경우 Instance 메서드를 생성합니다.

__MyClass3__ 처럼 람다식에 Outer 메서드의 변수를 사용하는 경우 Nested 클래스를 만들고, Nested 클래스 안에 Free Vaiable 필드를 추가하고 람다식을 메서드로 추가합니다.

<br><Br>

## __세대별 가비지 컬렉터 알고리즘__
<Br>

### __세대별 가비지 컬렉터__

게임 오브젝트를 동적 생성/삭제 하는것은 가비지 컬렉터의 활동 빈도를 높입니다.

가비지 컬렉터 실행의 비용은 공짜가 아니기 때문에 신중하게 관리되어야 합니다.

때문에 오래 참조되어 사용될 객체들을 모아놓고 세대를 나눠줌으로써 메모리를 효율적으로 관리할 수 있습니다.
<Br>

- __0세대__ : 아직 가비지 컬렉션이 수행되지 않은 객체들

- __1세대__ : 가비지 컬렉션이 1번 수행되고 살아남은 객체들

- __2세대__ : 가비지 컬렉션이 2번 수행되고 살아남은 객체들
<br><Br>



### __객체의 수명 예측은 어떻게?__


<img src="https://user-images.githubusercontent.com/80774412/227879438-988c52ce-874b-40b9-ac7b-e56237ac1e23.png"></img>

- __새로 힙에 할당된 객체들을 모두 0세대로 포함합니다.__

    - 0세대의 메모리가 임계치에 도달하면 0세대에 대해 가비지 컬렉션을 수행합니다.

    - 가비지 컬렉션 후 살아남은 객체들을 1세대로 옮겨집니다.<Br>

- __이후 또 다른 객체들이 할당되면 0세대로 포함합니다.__

    - 0세대가 또 임계치에 도달하면 가비지 컬렉션을 수행합니다.<Br>

- __1세대가 임계치에 도달하면 가비지 컬렉션을 수행합니다.__

    - 0, 1 세대 모두 가비지 컬렉션이 실행됩니다.

    - 0세대에서 살아남은 객체들은 1세대로, 1세대에서 살아남은 객체들은 2세대로 옮겨집니다.<Br>

- __2세대가 임계치에 도달하면 더 이상 다른 세대로 옮겨지지 않고 전체 가비지 컬렉션을 실행합니다.__


2세대 힙이 객체로 가득차게 되어(풀GC) 전체 힙에대한 가비지 컬렉션이 실행될 때 CLR이 해당 어플리케이션을 일시 중단하고 실행합니다.

어플리케이션이 차지하는 메모리 공간이 많을수록 중단 시간이 길어집니다.<BR><bR>

### __가비지 컬렉션 줄이는 방법__

1. 객체의 생성 및 할당을 너무 많이 하지 않습니다.

2. 대형 객체의 힙 할당은 메모리 공간 효율을 낮춥니다. 

3. 객체 간 참조관계를 줄입니다.

    참조관계가 많은 객체는 2세대 힙까지 살아남을 가능성이 큽니다.

    세대를 옮길 때 해당 객체의 모든 필드의 참조관계를 조사하고 참조 메모리 주소를 변경해야 하기 때문에 오버헤드가 발생합니다.

4. 루트(?) 목록을 줄입니다.

[참고 문서](https://luv-n-interest.tistory.com/922)
<br><Br>

## 📌 __*.meta 파일__

### __Unity Asset 내부 처리__

모든 에셋파일에는 대응하는 meta파일이 존재합니다.

저장되는 정보는 아래와 같습니다.

- 유니티 내부적으로 Asset을 참조하는데 사용되는 ID값

- Asset에 대한 import Settings 정보

    Project View에서 Asset을 클릭하면 Inspector에 나타나는 정보입니다.

    Asset의 종류에 따라 Import Settings도 다릅니다.

__에셋 추가 프로세스__

1. asset을 import 하거나 Asset 폴더에 새로운 파일을 생성하면

    asset에 unique한 ID를 부여합니다.

    이 ID는 unity 내부적으로 asset을 참조하는데 사용됩니다.

    이후 폴더를 옮기거나 이름을 변경해도 ID를 통해 파일을 참조할 수 있습니다.

2. *.meta 파일을 생성합니다.

    *.meta 파일은 asset이 존재하는 폴더에 생성됩니다.

    에셋 고유 ID는 *.meta 파일에 저장됩니다.

    __유니티 내에서__ asset의 위치나 파일명이 변경되면 *.meta 파일의 위치와 이름도 변경됩니다.

    하지만 __유니티 외부(윈도우)__ 에서 파일을 조작하면 수동으로 변경해 줘야 합니다.
<br><Br>


## 📌 __메타파일 누락을 조심해야 하는 경우__

- GIT으로 협업할 경우

    작업한 asset과 그 메타 파일도 함께 push 해야 합니다.

    메타파일이 누락된 파일을 다른 사람이 pull하고 유니티를 시작하면,

    각각의 asset마다 새로운 메타파일이 생성되고 다시 push하여 병합하려고 하면 오류가 발생합니다.

- 유니티 외부(카톡)로 파일을 공유할 경우

    이런 경우 UnityPackage형태로 파일을 공유하면 해결됩니다.

    Unity Package에는 메타 파일 정보도 모두 포함 되기 떄문입니다.

[참고 문서](https://coding-groot.tistory.com/5)

----

## __관련 지식__
<br>

### __메서드 시그니처__

매서드의 이름과 매개변수 리스트를 말합니다.

i.e. 오버로딩의 조건 [원문](#new-한정자)
<br><Br>

### __일급 함수__

함수를 일종의 데이터 타입처럼 취급할 수 있을 때,

즉 함수를 다른 메서드의 파라미터로 전달하여 사용할 수 있을 때 이를 일급함수라고 합니다.[원문](#closure는-어디에-사용하는가)
<Br><Br>