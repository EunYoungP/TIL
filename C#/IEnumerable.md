23.04.19

# __IEnumerable, IEnumerator : C# 문법__

[마이크로소프트 공식 문서_IEnumerable](https://learn.microsoft.com/ko-kr/dotnet/api/system.collections.ienumerable?view=net-8.0)

---

<BR><bR>

## __목차__

* [IEnumerable](#ienumerable)

* [IEnumerator](#ienumerator)

    *[IEnumerator 주의사항](#ienumerator-주의사항)

* [IEnumerator&IEnumerable 구현예시](#ienumerator-ienumerable-예시)

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

## __IEnumerator, IEnumerable 예시__

```c#
// 사람 한명을 나타내는 클래스
class Person
{
    public Person(string firstname, string lastname)
    {
        this.firstname = fistname;
        this.lastname = lastname;
    }

    string firstname;
    string lastname;
}

// 사람들을 나타내는 클래스
class People : IEnumerable
{
    Person[] people;

    public People(Person[] persons)
    {
        for(int i= 0; i < persons.Length; i ++)
        {
            people[i] = persons[i];
        }
    }

    // IEnumerable.GetEnumerator
    IEnumerator GetEnumerator()
    {
        // IEnumerator를 반환받습니다.
        return new PeopleEnum(people);
    }
}

class PeapleEnum : IEnumerator
{
    public Person[] people;
    int curPosition = -1;

    // IEnumerator.Current
    public object Current
    {
        get{return peaple[cirPosition];}
    }

    // IEnumerator.MoveNext
    public bool MoveNext()
    {
        curPosition++;
        return people.Length > curPosition;
    }

    // IEnumerator.Reset
    public void Reset()
    {
        curPosition = -1;
    }
}
```
<Br><Br>

## __foreach__

__foreach__ 문을 사용하려면 사용하려는 객체의 클래스가 IEnumerator 객체를 리턴하는 __GetEnumerator()__ 와 __Current, MoveNext, Reset__ 함수가 구현되어 있어야 합니다.

```c#
class app
{
    static void Main()
    {
        Person[] personArray = new Person[3] 
        {
            new Person("Eunyoung", "GeonUk"),
            new Person("Minji", "Suji"),
            new Person("Wonyoung", "Ujin")
        }

        People people = new People(personArray);
        foreach(Person person in people)
        {
            Console.WriteLine(person.firstname + person.lastname);
        }
    }
}
```

People 객체가 IEnumerable 인터페이스를 상속받아 GetEnumerator를 구현하고있고, IEnumerator를 상속받아 3가지 함수를 구현한 객체도 있기 때문에 foreach문을 통해 원소를 차레대로 리턴받을 수 있습니다.

foreach 코드를 실행하면 컴파일러가 아래와 같은 코드로 변경하여 실행하는 것처럼 됩니다.

```c#
IEnumerator enumerator = poeple.GetEnumerator();
while(enumerator.Movenext())
{
    Console.WriteLine(Current.firstnmae + Current.lastname);
}
```
<Br><Br>

## __yield__

yield 는 IEnumerator, IEnumerable 의 간편 표기법 입니다.

c#에서 yield 를 사용하면, 명시적으로 별도의 Enumerator 클래스를 작성할 필요가 없습니다.(i.e. 컴파일러가 자동으로 만들어 줍니다.)

반환형식이 IEnumerator, IEnumerable 인 함수는 yield 키워드를 이용해서 일시정지, 비동기 동작이 가능합니다.

## __yield 두가지 형식__

* yield return 

    * 반복에 값을 전달합니다.

    * 함수를 호출한 곳에 리턴을 해준 후 다시 돌아와서 yield return 문을 실행합니다.

    * 해당 함수가 끝날 때까지 기다리는것이 아닌, 함수 중간에 잠시 빠져나와 호출한 곳에 리턴 값을 전달해주고 다시 돌아와서 함수를 마저 진행합니다.

* yield brak

    * 반복 종료를 명시적으로 알립니다.
----

[참고 문서](https://ansohxxn.github.io/c%20sharp/enumerate/)
[마이크로 소프트 공식문서 yield](https://learn.microsoft.com/ko-kr/dotnet/csharp/language-reference/statements/yield)