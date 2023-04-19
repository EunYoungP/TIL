23.04.19

# __IEnumerable, IEnumerator : C# 문법__

[마이크로소프트 공식 문서_IEnumerable](https://learn.microsoft.com/ko-kr/dotnet/api/system.collections.ienumerable?view=net-8.0)

---

<BR><bR>

## __목차__

* [IEnumerable](#ienumerable)

* [IEnumerator](#ienumerator)

    *[IEnumerator 주의사항](#ienumerator-주의사항)

<br><Br>

## __IEnumerable__

IEnumerable 인터페이스는 __IEnumerator 를 반환__ 하는 단일 메서드인 GetEnumerator 가 포함되어 있습니다.

c#의 모든 컬렉션은 IEnumerable, IEnumerator를 상속받아 구현하고 있습니다.

IEnumerable 인터페이스를 구현한 클래스만이 foreach로 내부 데이터를 열거할 수 있습니다.

```c#
public interface IEnumerable
{
    IEnumerator GetEnumerator();
}
```

* GetEnumerator : 컬렉션 반복에 사용될 IEnumerator를 리턴받습니다.

<br><Br>

## __IEnumerator__

Current 속성, MoveNext, Reset 메서드로 __컬렉션을 반복하는 기능__ 을 제공하는 인터페이스 입니다.

```c#
public interface IEnumerator
{
    object Current(get;)
    bool MoveNext();
    void Reset();
}
```
* Current : 읽기 전용 프로퍼티 입니다. 현재 위치를 object 타입으로 리턴합니다.

* MoveNext : 다음 위치의 데이터가 있으면 true, 없으면 false를 반환합니다.

            컬렉션 인덱스를 1씩 증가 시켜 컬렉션의 끝에 도달 했는지 여부를 나타낼 수 있습니다.

* Reset : 인덱스를 초기 상태로 위치 시킵니다.<br><BR>

## __IEnumerator 주의사항__

* IEnumerator 를 리턴하는 함수는 매개변수에 ref, out 이 허용되지 않습니다.

* 또한 람다식에도 사용할 수 없습니다.

<Br><Br>