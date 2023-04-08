23.04.07

# __콜백 CallBack__

---

<BR><bR>

## __목차__

* [콜백이란?](#콜백이란)

    * [콜백 코드 구현](#콜백-코드-구현)

    * [Delegate 콜백 구현](#delegate-callback-구현)
<br><Br>


## __콜백이란?__

1. 다른 코드의 인수로서 넘겨주는 실행 가능한 코드입니다.

2. 호출을 당한 쪽에서 다시 호출한 쪽의 메서드를 호출하는 것입니다.

<br><BR>

### __콜백 코드 구현__

```c#
public class CallBackExample
{
    void Main()
    {
        Mother mother = new Mother();
        Son son = new Son();

        mother.GetSonToStudy(son);
    }
}

class Mother
{
    // 1
    public void GetSonToStudy(Son son)
    {
        son.Study(this);
    }

    // 3 
    public void FinishStudy()
    {
        Debug.Log("Study done");
    }
}

Class Son
{
    // 2 콜백을 위해 mother를 전달합니다.
    public void Study(Mother mother)
    {   
        // 호출한 클래스의 메서드를 사용합니다. -> 콜백
        mother.FinishStudy();
    }
}
```

피호출자(Son)가 호출자(Mother)의 메서드를 호출하기위해 인자로 Motehr를 전달 받습니다.<br><BR>

### __Delegate Callback 구현__
```c#
public class CallBackExample
{
    void Main()
    {
        Mother mother = new Mother();
        Son son = new Son();

        mother.GetSonToStudy(son);
    }
}

public delegate void StudyDelegate();

class Mother
{
    // 1
    public void GetSonToStudy(Son son)
    {
        StudyDelegate del = FinishStudy();
        son.Study(del);
    }

    public void FinishStudy()
    {
        Debug.Log("Study done");
    }
}

Class Son
{
    public void Study(studyDelegate del))
    {   
        // 델리게이트 메서드
        del();
    }
}
```
델리게이트 콜백을 사용하여 Mother 객체를 전달하지 않고 델리게이트에 메서드만 담아 전달해 사용할 수 있습니다.
