23.04.06

# __확장명 메서드 Extension Method : C# 문법__

[마이크로소프트 공식 문서](https://learn.microsoft.com/ko-kr/dotnet/csharp/programming-guide/classes-and-structs/extension-methods)

---

<BR><bR>

## __목차__

* [확장 메서드](#확장-메서드)

    * [주요 사용 방법](#확장-메서드-주요-사용)

    * [LINQ 확장 메서드](#linq-확장-메서드)

    * [System.String 확장 메서드](#systemstring-클래스-확장-메서드)

    * [사용 유의사항](#사용-유의사항)

* [일반적 사용 패턴](#일반적-사용-패턴)

    * [컬렉션 기능](#컬렉션-기능)

    * [미리 정의된 형식 확장](#미리-정의된-형식-확장)

<br><Br>

## __확장 메서드__

새 파생 형식을 만들거나 다시 컴파일하거나 원래 형식을 수정하지 않고,

기존 형식에 메서드를 추가 할 수 있습니다.

* __정적 메서드__ 이지만 확장 형식의 인스턴스 메서드인 것처럼 호출됩니다.

* 클래스, 인터페이스를 확장할 수 있지만 __재정의할 수는 없습니다.__

* __이름과 시그니처가 인터페이스/클래스의 메서드와 동일한__ 확장 메서드는 호출되지 않습니다.

* 확장 메서드는 인스턴스 메서드보다 우선 순위가 낮습니다.<br><BR>

## __확장 메서드 주요 사용__

1.  메서드를 추가할 수 없는 __구조체(enum)__ 형식의 타입도 확장 메서드를 이용할 수 있습니다. 구조체의 함수가 존재하는 것처럼 사용할 수 있습니다.

2. 클래스의 함수를 호출할 때 해당 클래스 인스턴스의 null 체크를 진행해야할 경우 사용하면 코드가 명확해집니다.

```c#
// 클래스에 선언된 일반 함수입니다.
class Block
{
    public void Exemple1()
    {
        Console.WirteLine("클래스의 멤버 함수 입니다.");
    }
}

// 확장 메서드입니다.
public static void Exemple2(Block block)
{
    if(block == null)
        return;

    Console.WriteLine("확장 메서드입니다.");
}

public void main()
{
    Block block;

    // 1. null 체크를 메인 코드에서 실행해야 합니다.
    if(block!= null)
        block.Exemple1();

    // 2. block 객체의 null 체크를 하지않고 확장 메서드 안에서 진행할 수 있습니다.
    block.Exemple2();
}
```
<br><Br>

### __Linq 확장 메서드__

가장 일반적인 확장명 메서드는 쿼리 기능을 System.Collections.IEnumerable 및 System.Collections.Generic.IEnumerable<T> 형식에 추가하는 LINQ 표준 쿼리 연산자입니다.

IEnumerable<T>를 구현하는 모든 형식에

* GroupBy
* OrderBy
* Average

등의 인스턴스 메서드가 있는 것처럼 나타납니다.<BR><br>

### __System.String 클래스 확장 메서드__

이 확장 메서드는 제네릭이 아닌 비중첩 클래스 내부에서 정의됩니다.

```c#
namespace ExtensionMethos
{
    public static class MyExtensions
    {
        public static int WordCount(this string str)
        {
            return str.Split(new char[] {',', '.', '?"}, StringSplitOptions.RemoveEmptyEntries).Length;
        }
    }
}
```
<br><Br>
### 위 확장 메서드 사용법은 아래와 같습니다.

```c#
// 1
using ExtensionMethods;

// 2
string s = "Hello Extension Mehthods";
int i = s.WordCount();

//3
string s = "Hello Extension Methods"
int i = MyExtensions.WordCount(s);
```
<Br>

### __사용 유의사항__

* 시그니처가 형식에 이미 정의된 메서드와 동일한 확장 메서드는 호출되지 않습니다.

* 확장 메서드는 네임스페이스 수준에서 범위로 가져옵니다.

    e.g. Extensions 라는 네임스페이스에 확장 메서드를 포함하는 여러 개의 정적 클래스가 있는 경우 "using Extensions;" 지시문을 통해 모두 범위로 가져옵니다.
<Br><BR>

## __일반적 사용 패턴__

### __컬렉션 기능__

과거에는 지정된 형식에 대한 인터페이스를 해당 형식의 컬렉션에 작동하는 기능을 포함하는 __컬렉션 클래스__ 에 구현하였습니다.

하지만 인터페이스 확장을 사용하여 동일한 기능을 구현할 수 있습니다.

* 해당 형식에 대한 인터페이스를 구현하는 모든 컬렉션에서 기능을 호출할 수 있습니다.<br><Br>

### __미리 정의된 형식 확장__

재사용 가능한 기능을 만들어야 할 경우 새로운 개체를 만드는 대신 기존 형식(.NET, CLR)을 확장하여 사용할 수 있습니다.

<br>

만약 SQL 서버에 대해 쿼리를 실행해야 할 경우,

Query 클래스를 만들어 곳곳에서 호출하여 사용할 수 있습니다.

반면 __확장 메서드__ 를 사용하여  System.Data.SqlClient.SqlConnection 클래스를 확장하면 SQL 서버에 연결된 모든 곳에서 해당 쿼리를 수행하는 함수를 호출할 수 있습니다.

이것은 System.Stirng, System.IO.File, System.IO.Stream 등에 동일하게 적용됩니다.
<BR><bR>



