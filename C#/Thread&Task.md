23.03.09

📗 __'이것이 C# 이다' 의 일부 내용을 정리한 문서입니다.__<BR><BR>

# __스레드와 테스크 Thread & Task__

<Br>

운영체제는 여러가지 프로세스를 동시에 실행할 수 있게 해줍니다.

프로세스란 실행파일을 실행시켜 메모리에 적재된 인스턴스이므로, 즉 실행중인 프로그램을 의미합니다.

운영체제가 여러 작업을 동시에 하는 것처럼 프로세스도 여러 작업을 할 수 있습니다.

(e.g. 워드프로세스에서 글을 작성하는 동안 맞춤법 검사를 실행합니다.)
<br><br>


## 💡 __Thread 란?__

__프로세스__ 는 반드시 한 개 이상의 __스레드__ 로 구성됩니다.

__스레드__ 는 운영체제가 __CPU 시간을 할당하는 기본 단위__ 입니다.

대부분의 예제 프로그램들은 하나의 스레드를 갖는 __단일 스레드__ 구조를 갖고 있었습니다.

지금 부터 여러개의 스레드를 가지는 __멀티 스레드__ 구조의 프로그램을 알아야 합니다.
<Br><Br>

## 📌__Thread 의 장점__

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


## 📌__Thread 의 단점__

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

## __스레드 임의 종료__

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


### ___Thread 상태 규칙___

<img src="https://user-images.githubusercontent.com/80774412/224059713-c40cb064-d63c-4f7d-9301-cb4cae029a16.png"></img>

Abort  -- x -->  Running

Running -- x --> Unstarted

Background 상태는 전이되는 것이 아닌, 그저 스레드가 어떤 상황에 처해 있는지에 관한 정보이기 때문에 위 이미지 전환 과정에 표기되어 있지 않습니다.

----

## ___관련 지식___<br><Br>

#### ___비트 필드 Bit field___

C언어 개발 초기 1 바이트로 0~255 까지 표현할 수 있는데 기껏해야 0, 1, 2 등의 값을 갖는 플래그를 표현하려고 한 바이트를 낭비하는 것에서 비롯되어 만들어진 플래그입니다.

C# 이 개발된 메모리가 풍부해진 시기에는 사용할 필요가 없었지만, C 개발자들이 .NET 프레임워크에 사용할 수 있도록 Flags 애트리뷰트를 선언해 뒀습니다.
[원문](#threadstate-열거형)<Br><Br>


