23.03.09

# __스레드와 테스크 Thread & Task__

📗 __'이것이 C# 이다' 의 일부 내용을 정리한 문서입니다.__<BR>

## __목차__

__Thread__

- [Thread란?](#💡-thread-란)

- [Thread 사용하기](#📌-thread-사용하기)

- [Thread 장점](#📌__thread-의-장점__)

- [Thread 단점](#📌-thread-의-단점)

- [Thread 임의 종료법: Abort](#📌-스레드-임의-종료-방법-1-abort)

- [Thread 임의 종룝법: Inturrupt](#📌-스레드-임의-종료-방법-2-인터럽트)

- [Thread 상태변화](#📌-스레드의-상태변화)

- [Thread 동기화](#📌-스레드-간의-동기화)
<Br>

__TASK__



[Thread Task 차이점](#thread-와-task-차이)

<br><Br>

## 💡 __Thread 란?__

운영체제는 여러가지 프로세스를 동시에 실행할 수 있게 해줍니다.

프로세스란 실행파일을 실행시켜 메모리에 적재된 인스턴스이므로, 즉 실행중인 프로그램을 의미합니다.

운영체제가 여러 작업을 동시에 하는 것처럼 프로세스도 여러 작업을 할 수 있습니다.

(e.g. 워드프로세스에서 글을 작성하는 동안 맞춤법 검사를 실행합니다.)<br><Br>

__프로세스__ 는 반드시 한 개 이상의 __스레드__ 로 구성됩니다.

__스레드__ 는 운영체제가 __CPU 시간을 할당하는 기본 단위__ 입니다.

대부분의 예제 프로그램들은 하나의 스레드를 갖는 __단일 스레드__ 구조를 갖고 있었습니다.

지금 부터 여러개의 스레드를 가지는 __멀티 스레드__ 구조의 프로그램을 알아야 합니다.
<Br><Br>

## 📌 __Thread 의 장점__

1. __대화형 프로그램에서 응답성을 높일 수 있습니다 :__

    파일을 복사하는 중간에 취소를 하고 싶을 경우 명령을 입력받을 수 있도록 
    합니다.

2. __멀티 프로세스 방식보다 자원 공유가 쉽습니다.__

    - 멀티 프로세스(e.g. GUI 없는 웹 서버 어플리케이션)

        데이터 교환을 위해 소켓, 공유 메모리같은 IPC를 이용해야 합니다.

    - 멀티 스레드

        데이터 교환을 위해 스레드끼리 코드 내의 변수를 같이 사용하면 됩니다.

3. __경제성__

    프로세스를 띄우는 작업은 메모리와 자원을 할당하는 CPU 사용 시간 등 비용이 비쌉니다.

    스레드를 띄울 떄는 이미 프로세스에 할당된 메모리와 자원을 그대로 사용하므로 비용을 지불하지 않아도 됩니다.
<BR><bR>


## 📌 __Thread 의 단점__

1. __까다로운 구현과정__

    구현하기가 어렵고 테스트, 디버깅이 쉽지 않습니다.

2. __의존성__

    __멀티 프로세스__ 방식은 하나의 프로세스에 문제가 생기면 프로세스 하나만 죽는 것 이상으로 문제가 생기지 않습니다.

    그에 반해 __멀티 스레드__ 기반의 방식은 하나의 스레드에 문제가 생겨도 전체 프로세스가 영향을 받게 됩니다.

3. __성능 저하__

    장점으로 성능을 꼽았으나, 스레드를 너무 많이 사용하면 오히려 성능이 저하됩니다.

    스레드가 CPU 를 사용하기 위한 __작업간 전환__ 이 비용을 많이 소모하기 떄문에,
    작업간 전환을 너무 자주 수행하면 실제 일을 하는 시간보다 작업간 전환 시간이 커집니다.
<bR><BR>

멀티 스레드 구조를 취하느냐 마느냐에 대해서는 의견이 분분합니다.

성능이 좋아지지만 안정성이 떨어질 수 있기 때문입니다.
<BR><bR>

## 📌 __Thread 사용하기__

__.NET__ 에서는 __System.Threading.Thread__ 클래스를 제공합니다.<BR><BR>

### __Thread 클래스 사용법__

__1. Thread 인스턴스 생성__ : 생성자의 매개변수로 스레드가 실행할 메소드를 넘깁니다.

__2. Thrad.Start() 메소드 호출__ : 스레드를 시작합니다.

__3. Thread.Join() 메소드 호출__ : 스레드가 끝날 때까지 기다립니다.
<br><Br>

### __Thread 클래스 사용법 코드변환__

```c#
// 스레드가 실행할 메소드
static void DoSomething()
{
    for(int i = 0; i < 5; i++)
    {
        Conosole.WhriteLine("DoSomething : {0}", i);
    }
}

static void main(string[] args)
{
    // 1단계. Thread 인스턴스 생성
    Thread t1 = new Thread(new ThreadStart(DoSomething));

    // 2단계. Thread 시작
    t1.Start();

    // 3단계. Thread 종료 대기
    t1.Join();
}
```

실제 스레드가 메모리에 적재되는 시점 : 2단계

1단계는 준비만 해둘 뿐입니다.

3단계는 DoSomething 메소드가 끝나면 반환되어 다음 코드가 실행될 수 있게 합니다.
<br><Br>

Join 은 '합류하다' 라는 의미를 가지고 있으므로 다음 코드 시점으로 합류된다고 생각하면 쉽습니다.<br><Br>


```c#
using System
using System.Threading;

namesapce BasicThread
{
    class MainApp
    {
        static void DoSomething()
        {
            // t1 스레드 반복문
            for(int i = 0; i < 5; i++)
            {
                Console.WriteLine($"DoSomething : {i}");
                // Sleep() 메소드: 다른 스레드가 CPU를 사용할 수 있도록
                // 점유를 내려놓습니다. 매개 변수는 밀리초 단위입니다.
                Thread.Sleep(10);
            }
        }

        static void main(string[] args)
        {
            // 1. 스레드 인스턴스
            Thread t1 = new Thread(new ThreadStart(DoSomething));

            // 2. 스레드 시작, 메모리 적재
            t1.Start();

            // 메인스레드 for문
            for(int i = 0; i < 5; i++)
            {
                // 3-1. 
                Console.WriteLine(&"Main : {i}");
                Thread.Sleep(10);
            }

            // 4. 스레드 종료 대기
            Console.WriteLine("Wating until thread stops...");
            t1.Join();
            
            // 5. 모든 코드 흐름 끝
            Consnole.WriteLine("Finished");
        }
    }
}
```
위 코드의 결과는

인스턴스가 생성되고 t1 스레드가 시작된 후 메인 스레드와 t1 스레드의 for문이 함께 실행됩니다. 그리고 t1.Join 으로 t1 스레드의 흐름이 메인 스레드로 합류합니다.
<BR><bR>

## 📌 __스레드 임의 종료 방법 1: Abort__

프로세스는 사용자가 작업 관리자 등으로 읨의로 죽일 수 있습니다.

스레드는 Abort() 메소드를 호출하여 가능합니다.

```c#
static void DoSomething()
{
    try
    {
        for(int i = 0; i < 5; i++)
        {
            Console.WriteLine(&"{i}");
        }
    }
    catch(ThreadAbortedException)
    {

    }
    finally
    {
    }
}

static void main(string[] args)
{
    Thread t1 = new Thread(ThreadStart(DoSomething));

    t1.Strat();
    t1.Abort();
    t1.Join();
}
```

Abort 메서드가 호출된다고 스레드가 즉시 종료된다는 보장은 없습니다.

Abort 메서드 호출 시 ThreadAbortedException 이 호출되어 예외 처리를 한 다음 스레드가 완전히 종료됩니다.

Abort 의 사용은 아래이유 때문에 거의 사용되지 않습니다.

- Abort 단점

    한 스레드가 특정 자원을 독점하고자 잠근 후 그 자원을 해제하지 못한 채 Abort 가 실행되면,

    해당 자원에 접근하고자 하는 다른 스레드들은 그대로 진행되지 못하는 신세가 됩니다.

<br><Br>

## 📌 __스레드 임의 종료 방법 2: 인터럽트__

스레드는 스스로 종료하는 것이 가장 좋지만 불가피하게 강제 종료시켜야 하는 경우

Abort 보다 부드러운 방법인 Thread.Interrupt() 메소드가 존재합니다.

스레드가 실행중인 상태(Running) 를 피해 WaitJoinSleep 상태일 경우 ThreadInterruptedException 예외를 던져 스레드를 중지시킵니다.

WaitJoinSleep 상태가 아닐 경우 WaitJoinSleep 상태가 될 때까지 대기하다가 나중에 중단시키기 때문에 "절대 중단되면 안되는 작업" 을 하고있을 경우 보장 받을 수 있습니다.

사용법은 Abort 메소드와 같습니다.

<br><Br>


## 📌 __스레드의 상태변화__

### ___ThreadState 열거형___

|형태   | 설명  | 
|:---:|:---|
|Unstarted |스레드 객체가 생성되고 Thread.Start 메서드가 호출되기 전 상태  |
|Running|Thread.Start 메서드가 호출되어 실행중인 상태  |
|Suspended|스레드의 일시중단 상태입니다. Thread.Suspend() 메서드를 통해 이 상태로 만들 수 있고, Thread.Resume() 메소드를 통해 다시 Running 상태로 만들 수 있습니다.  |
|WaitSleepJoin|스레드가 블록된 상태입니다.Thread.Join, Thread.Sleep, Monitor.Enter 메서드를 통해 이 상태로 만들 수 있습니다.  |
|Aborted|스레드가 취소된 상태를 나타냅니다. Trhead.Abort 메서드를 호출하면 이상태가 되고 Stopped 상태로 전환됩니다.  |
|Stopped|스레드가 중지된 상태를 나타냅니다. Abort 메소드를 호출하거나 스레드가 종료되면 이 상태가 됩니다.  |
|Background|스레드가 백그라운드로 동작하고 있음을 나타냅니다. 즉, 프로세스가 종료되면 백그라운드 스레드가 열 개 살아있어도 모두 죽습니다. Thread.IsBackground 속성에 true 값을 입력해 이 상태로 바꿀 수 있습니다. |

ThreadState 열거형은 Flags 애트리뷰트를 가지고 있습니다.

__Flags__ 는 자신이 수식하는 열거형을 [비트 필드](#비트-필드-bit-field)(플래그 집합) 으로 처리할 수 있음을 나타냅니다.
<br><Br>

## __ThreadState Flags__

```c#
enm MyEnum
{
    Apple,  //0
    Banana, //1
    Kiwi,   //2
    Mango   //3
};
```
||일반 열거형|Flags 애트리뷰트 사용 열거형|
|---|---|---|
|(MyEnum)0|Apple|Apple|
|(MyEnum)1|Banana|Banana|
|(MyEnum)2|Kiwi|Kiwi|
|(MyEnum)3|Mango|Mango|
|(MyEnum)4|4|Apple, Mango|
|(MyEnum)5|5|Banana, Mango|

위와 같이 Flag 애트리뷰트는 열거형의 요소들의 집합으로 구성되는 값들도 표현할 수 있습니다.

스레드는 한 번에 두가지 이상의 상태를 가질 수 있습니다.

(e.g. Suspended-WaitSleepJoin, Bakcground-Stopped)

따라서 두 가지 이상의 상태를 표현하기 위해 Flags 애트리뷰트가 수식되어 있습니다.<br><Br>

<img src="https://user-images.githubusercontent.com/80774412/224069707-ef01be46-8f7b-4216-b8e4-ac8407798541.png"></img>

보통의 열거형처럼 1씩 증가하지 않고 2의 제곱으로 증가하는 이유는 비트 연산을 통해 어떤 상태에 있는지 쉽게 알아낼 수 있기 때문입니다.

따라서 ThreadState 필드를 통해 상태 확인 시 반드시 비트 연산을 이용해야 합니다.

    if(t1.ThreadState & ThreadState.Aborted == ThreadState.Aborted)
        Console.WirteLine("스레드가 정지했습니다.");

<Br><Br>

### ___Thread 상태 규칙___

<img src="https://user-images.githubusercontent.com/80774412/224059713-c40cb064-d63c-4f7d-9301-cb4cae029a16.png"></img>

Abort  -- x -->  Running

Running -- x --> Unstarted

Background 상태는 전이되는 것이 아닌, 그저 스레드가 어떤 상황에 처해 있는지에 관한 정보이기 때문에 위 이미지 전환 과정에 표기되어 있지 않습니다.<br><Br>


## 📌 __스레드 간의 동기화__

스레드는 여러 가지 자원을 공유합니다.(e.g. 파일 핸들, 네트워크 커넥션, 메모리에 선언한 변수)

스레드는 다른 스레드가 사용 중인 자원을 멋대로 사용하는 기질을 가지고 있습니다.

__동기화__ 란 __스레드들이 순서대로 자원을 사용하게 하는 것__ 을 의미합니다.

즉 '자원을 한번에 하나의 스레드가 사용하도록 보장' 하는 것입니다.

동기화 도구로는 __lock__ 키워드와 __Monitor__ 클래스가 있습니다.<br><Br>

## ___lock 동기화___

lock 키워드는 평범함 코드를 __크리티컬 섹션__ 으로 바꿔줍니다.

* _크리티컬 섹션: 한번에 한 스레드만 사용할 수 있는 코드 영역_

```c#
class MyClass
{
    int count = 0;
    
    public void Increase()
    {
        count = count + 1;
    }
}

class MainApp
{
    static void Main(string[] args)
    {
        MyClass myclass = new Myclass();

        Thread t1 = new Thread(ThreadStart(myclass.Increase));
        Thread t2 = new Thread(ThreadStart(myclass.Increase));
        Thread t3 = new Thread(ThreadStart(myclass.Increase));

        t1.Start();
        t2.Start();
        t3.Start();

        t1.Join();
        t3.Join();
        t2.Join();
    }
}
```
위 코드의 결과는 3 일것 같지만, 사실은 실행할 떄마다 1, 2, 3 중 어떤 값이 나올지 모릅니다.

count = count + 1 은 간단해 보이지만 내부적으로 여러 단계의 하위 연산으로 나뉘는 코드입니다.

즉 t1 의 스레드가 시작되어 코드를 실행하는 도중 t2가 실행될 수도 있고 t3 도 마찬가지 입니다.

이 문제를 해결하기 위해서 count = count + 1 코드가 실행중일 때 다른 스레드가 실행하지 못하도록 하는 크리티컬 섹션이 필요합니다.

```c#
int count = 0;
pirvate readonly object thisLock = new Object();

public void Increase()
{
    // lock 키워드와 중괄호로 감싸진 부분이 크리티컬 섹션이 됩니다.
    // lock 블록이 끝나는 괄호를 만나기 전까지 다른 스레드는 절대 이 코드를 실행할 수 없습니다.
    lock(thisLock)
    {
        count = count + 1;
    }
}
```
lcok 키워드의 매개변수는 public 이나 외부에서 접근할 수 있는 경우 사용하지 않는 것이 좋습니다.

다른 스레드가 하나의 락 매개변수를 얻기 위해 대기해야 하는 상황이 일어날 수 있기 때문입니다.

_e.g. this, Type 형식(typeof, GetType()), string 형식_<br><Br>

## ___Monitor 동기화___

__Monitor__ 클래스는 스레드 도기화에 사용하는 정적 메소드를 제공합니다.

- __Monitor.Enter()__: 크리티컬 섹션을 만듭니다.

- __Monitor.Exit()__: 크리티컬 섹션을 제거합니다.

두 메소드는 lock 키워드와 같은 역할을 합니다.

```c#
int count = 0;
pirvate readonly object thisLock = new Object();

public void Increase()
{
    // 크리티컬 섹션 생성
    Monitor.Enter(thisLock);
    try
    {
        count = count + 1;
    }
    finally
    {
        // 크리티컬 섹션 해지
        Monitor.Exit(thisLock);
    }
    Thread.Sleep(1);
}
```

사실 lock 키워드는 이 두 메소드를 바탕으로 구현되어 있기 때문에,

Exit 호출 위치 오류, 호출 부재로 인한 버그가 생길 가능성을 lock 사용으로 줄일 수 있습니다.<br><Br>


## __저수준 동기화__

- __Monitor.Wait()__
- __Monitor.Pulse()__

Enter-Exit 보다 Wait-Pulse 메소드가, lock 키워드를 사용했을 때 보다 더 섬세하게 멀티 스레드 간의 동기화를 가능하게 해줍니다.

lock 블록 안에서 호출해야 합니다. 그렇지 않으면 SynchronizationLockException 예외가 던져집니다.<br><Br>

### __사용 순서__

최소 요구사항인 lock 블록이 필요합니다.

<img src="https://user-images.githubusercontent.com/80774412/224244996-811504f6-3583-4f4a-b722-3fc0ef43de8e.png" title = "https://www.shekhali.com/c-monitor-class-in-multithreading-with-examples/"></img>

이 과정에서는 __Waiting Queue__ 와 __Ready Queue__ 가 존재합니다.

1. 스레드가 __lock__ 을 소유한 상태가 됩니다.

2. __Wait()__ 메소드를 통해 스레드는 lock 을 해제 후 __WaitSleepJoin__ 상태로 진입합니다.

3. __Waiting Queue__ 에 스레드가 입력됩니다.

4. lock 을 넘겨받은 스레드2 가 작업을 수행이 끝나 __Pulse()__ 메소드를 호출합니다.

5. Waiting Queue 의 첫 요소를 꺼내 __Ready Queue__ 에 입력합니다.

6. Ready Queue 에 입력된 차례에 따라 lock을 얻어 __Running__ 상태에 들어갑니다.
<Br><Br>

Thread.Sleep 또한 WaitSeelpJoin 상태로 만들지만, Pulse() 메소드에 의해 꺠어날 수 없기 떄문에 Wait() 함수를 사용합니다.<br><Br>

## __사용법__








<br><Br>

## __Thread 와 Task 차이__

Thread 는 Task를 처리하는 가장 작은 단위입니다.

Thread는 메모리 상의 Task를 유지시키면서 사용자가 요구하는 서비스를 수행합니다.

비유하자면 __Thread__ 가 __일꾼__ 이라면 __Task__ 는 __일감__ 이고 __Process__ 는 __노동조합__ 입니다.

아래 프로그램 동작을 설명하며 Thread와 Task를 알아보겠습니다.<bR><bR>

___윈도우 운영체제 프로그램 동작 절차___

1. 프로그램을 실행합니다.

2. 프로그램이 프로시저로 변하고 메모리에 올라갑니다.

3. Main Thread 에서 Message Queue를 생성합니다.

4. CLR로부터 __Thread Pool__ 을 할당받습니다.
<br><Br>

중요한 것은 4번 과정인데, 프로세스 당 하나의 __Thread Pool__ 이 존재합니다.

- __차이점 1. Background Thread__

    즉 Thread를 사용하기 위해 Thread 객체를 만들어 사용하지 않아도 Thread Pool에 있는 Thread를 가져오는 방법으로 스레딩을 할 수 있습니다.

    이 방법이 바로 __Task-async-await__ 를 사용하는 __TAP__ 방법입니다.

    Task를 통해 Thread Pool 에서 Thread를 호출하는 것은 기본적으로 Background Thread들이기 때문에 

    프로그램이 죽게되면 Thread가 모두 죽습니다.

    따라서 다음에 프로그램을 다시 실행할 때 문제없이 실행된다는것을 의미합니다. [좀비 프로세스](#좀비zombiedefunct-프로세스) 가 생기지 않습니다.<Br><Br>

- __차이점 2. Thread 객체 할당 비용__

    물론 Thread 객체를 생성하여 호출해도 IsBackground 속성을 true로 만들면 Background로 동작하지만, Task 로 Thread를 호출하면 작업이 완료되었을 때 다시 Thread Pool에 Thread를 반환한다는 차이점이 있습니다.

    즉 Thread 객체를 만들어 사용하면 Abort시켜 재할당해서 사용해야 하기 때문에 할당 시스템 자원이 들어갑니다.

    하지만 Task로 Thread Pool에있는 Thread를 가져다 쓰면 작업이 완료되면 다시 반환하기 때문에 자원이 들어가지 않습니다.<br><Br>

    주의할 점은 ThreadPool 의 기본 크기보다 더 많은 Thread를 동시에 사용하는 경우입니다.

    이런 경우 Thread Pool이 Thread를 자동 생성하여 오버헤드가 발생하여 Thread 객체를 생성하여 사용할 때보다 느려지게 됩니다.
<BR><bR>


[참고 자료 1](https://talkingaboutme.tistory.com/entry/Study-Task-Thread)

[참고 자료 2](https://christian289.github.io/dotnet/Using-Thread-or-Using-Task/)

<br><Br>

## __비동기성, 동기성__

- __비동기성__

    해야할 일을 위임하고 대기하는 방식입니다.

    e.g. 팀장이 팀원들에게 일을 배분하고 보고만 받습니다.

- __동기성__

    순차적으로 일을 끝내 나가는 방식입니다.

    e.g. 팀장이 팀원 한사람씩 일을 마칠때까지 지켜보며 기다립니다.
<Br><Br>

## __병렬성, 동시성__

<img src="https://user-images.githubusercontent.com/80774412/225408422-0b7df63f-5b44-4160-a99d-3af5116d9ceb.png"></img>

- __병렬성__

    2개 이상의 코어에서 동시에 여러 작업을 처리하는 것입니다.

    멀티 코어에서 멀티 스레드를 동작시키는 방식을 의미합니다.

- __동시성__

    1개의 코어에서 여러 작업을 빠르게 교차하여 동시에 진행되는 것처럼 보이게 하는 것입니다.

    싱글 코어에서 멀티 스레드를 동작시키기 위한 방식을 의미합니다.


[참고 자료 1](https://velog.io/@brown_eyed87/220929%EB%B3%91%EB%A0%AC%EC%84%B1%EB%8F%99%EC%8B%9C%EC%84%B1-%EB%B9%84%EB%8F%99%EA%B8%B0%EB%8F%99%EA%B8%B0)

<br><Br>

## __병렬성과 동기성의 비교__

<img src="https://user-images.githubusercontent.com/80774412/225434544-aebed0d0-677b-480d-b7a7-580cbf682d8c.png"></img>

둘 다 멀티태스킹 느낌이지만

__병렬성__ 은 쓰레드나 프로세스가 2개 이상의 작업을 동시에 진행하는 것이고,

__비동기성__ 은 이전 작업의 결과를 기다리지 않고 실행되는 것 뿐입니다.



## __쓰레드의 종류__

___1. 물리적인 쓰레드___

<img src="https://user-images.githubusercontent.com/80774412/225432123-c4400bbc-d69c-4dc3-a009-f1fcc2d4e04a.png"></img>

물리적인 코어를 논리적으로 쪼갠 논리적 코어입니다.

코어가 2개 뿐이라면, CPU가 컴퓨터를 속여 코어가 4개인 것처럼 작업을 진행하는 형식입니다.

병렬처리도 4개까지 가능합니다.<BR><bR>

___2. 논리적인 쓰레드___

<img src="https://user-images.githubusercontent.com/80774412/225432597-95bf6c0c-9f19-4540-a43e-c60c0526e3df.png"></img>

프로세스 내부에서 실행되는 세부적인 단위입니다.

논리적인 쓰레드는 프로그램에서 생성할 수 있는 쓰레드입니다.<bR><bR>

병렬로 실행될 수 있는 개수는 __(코어 개수) X (물리적 스레드 개수)__

나머지 스레드는 빠르게 교차되어 실행되거나 유휴상태로 남아있습니다.<BR><bR>

----

## ___관련 지식___<br><Br>

#### ___비트 필드 Bit field___

C언어 개발 초기 1 바이트로 0~255 까지 표현할 수 있는데 기껏해야 0, 1, 2 등의 값을 갖는 플래그를 표현하려고 한 바이트를 낭비하는 것에서 비롯되어 만들어진 플래그입니다.

C# 이 개발된 메모리가 풍부해진 시기에는 사용할 필요가 없었지만, C 개발자들이 .NET 프레임워크에 사용할 수 있도록 Flags 애트리뷰트를 선언해 뒀습니다.
[원문](#threadstate-열거형)<Br><Br>

#### ___좀비(Zombie/Defunct) 프로세스___

프로세스가 종료되고 리소스는 모두 회수되었지만

시스템 프로세스 테이블에 남아있는 defunct 상태의 프로세스를 __좀비 프로세스__ 라고 합니다. 

쉽게 말하면 __실행이 종료되었지만 아직 삭제되지 않은 프로세스__ 를 의미합니다.
[원문](#thread-와-task-차이)<BR><bR>


