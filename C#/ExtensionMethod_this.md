23.03.20

# __확장명 메서드 Extension Method : C# 문법__

[마이크로소프트 공식 문서](https://learn.microsoft.com/ko-kr/dotnet/csharp/programming-guide/classes-and-structs/extension-methods)

---

<BR><bR>

## __목차__

* [확장 메서드](#확장-메서드)

    * [LINQ 확장 메서드](#linq-확장-메서드)

    * [System.String 확장 메서드](#systemstring-클래스-확장-메서드)

* [컬렉션 기능](#컬렉션-기능)

<br><Br>

## __확장 메서드__

새 파생 형식을 만들거나 다시 컴파일하거나 원래 형식을 수정하지 않고,

기존 형식에 메서드를 추가 할 수 있습니다.

* __정적 메서드__ 이지만 확장 형식의 인스턴스 메서드인 것처럼 호출됩니다.

* 클래스, 인터페이스를 확장할 수 있지만 __재정의할 수는 없습니다.__

* __이름과 시그니처가 인터페이스/클래스의 메서드와 동일한__ 확장 메서드는 호출되지 않습니다.

* 확장 메서드는 인스턴스 메서드보다 우선 순위가 낮습니다.<br><BR>

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
<Br><BR>

## __컬렉션 기능__

과거에는 지정된 형식에 대한 인터페이스를 해당 형식의 컬렉션에 작동하는 기능을 포함하는 __컬렉션 클래스__ 에 구현하였습니다.

하지만 인터페이스 확장을 사용하여 동일한 기능을 구현할 수 있습니다.

* 해당 형식에 대한 인터페이스를 구현하는 모든 컬렉션에서 기능을 호출할 수 있습니다.


