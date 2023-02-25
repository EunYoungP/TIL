2023.02.25

# __큐 구현하기__

큐 는 FIFO (First In First Out) 자료구조입니다.

출입구가 단 한 군데인 스택과는 달리, 큐는 입구인 rear 와 출구인 front 가 존재합니다.

pop 을 실행하면 front 에 있는 노드를 빼내게 됩니다.

스택과는 완전히 반대되는 구조를 가지고 있어 사용하는 곳도 다릅니다.

큐는 주로 __순서대로 처리해야 하는 일__ 을 위해 사용합니다.(e.g. BFS)

스택과 마찬가지로 삽입, 삭제를 시간복잡도 O(1)을 가지게 하기 위해서, 연결리스트를 이용하여 큐를 구현해봤습니다.<BR><BR>


## __큐의 구조__

<img src="https://user-images.githubusercontent.com/80774412/221362276-3416ec13-6593-449b-9492-4d5ddac68cb1.png"></img>

## __CPP 큐 구현__

```C++
// 큐의 동작을 출력하기위한 헤더파일
#include <iostream>
using namespace std;

// 언더플로우 예외처리를 하기 위한 클래스
class UnderFlowException {};

template<typename T>
class ListNode
{
public:
    // 노드 자신이 가지고있을 값
    int value;
    // 노드가 가리킬 다음 노드
    ListNode<T>* next;

    ListNode<T>() : next(nullptr) {}
    ListNode<T>(int _value): value(_value), next(nullptr){}
};

template<typename T>
class Queue
{
public:
    // 큐를 구성하는 노드의 총 개수
    int size;
    // 맨 앞의 노드 
    ListNode<T>* head;
    // 맨 끝의 노드
    ListNode<T>* rear;

    Queue<T>() : size(0), head(nullptr), rear(nullptr){}

    // 모든 노드의 메모리를 해제
    ~Queue<T>()
    {
        ListNode<T>* temp = head;
        while (temp != nullptr)
        {
            head = head->next;
            delete(temp);
        }
    }

    // 큐의 rear 에 value 값을 추가합니다.
    void push(T value)
    {
        if (size == 0)head = rear = new ListNode<T>(value);
        else
        {
            rear->next = new ListNode<T>(value);
            rear = rear->next;
        }
        size++;
    }

    // 큐의 front에 존재하는 값을 삭제합니다.
    void pop()
    {
        try 
        {
            if (size == 0)throw  UnderFlowException();

            ListNode<T>* temp = head;
            head = head->next;
            delete(temp);

            if (head == nullptr)
            {
                
            }

            size--;
        }
        catch (UnderFlowException e)
        {
            cout << "큐가 비어있는데 pop 연산을 시도했습니다." << endl;
            exit(1);
        }
    }

    // 큐의 front 에 있는 값을 반환합니다.
    T front()
    {
        try
        {
            if (size == 0) throw UnderFlowException();

            return head->value;
        }
        catch (UnderFlowException e)
        {
            cout << "큐가 비어있는데 front 연산을 시도했습니다." << endl;
            exit(2);
        }
    }
};

// 큐의 모든 값을 나열하여 출력, 형식: [ queue의 값들 ]
template<typename T>
ostream& operator <<(ostream& out, Queue<T>& q)
{
    ListNode<T>* temp = q.head;
    out << "front [ ";
    for (int i = 0; i < q.size; i++)
    {
        out << temp->value << " ";
        temp = temp->next;
    }
    out << "] rear";

    return out;
}

int main()
{
    Queue<int> q;
    q.push(0); cout << q << endl;
    q.push(1); cout << q << endl;
    q.push(2); cout << q << endl;
    q.push(3); cout << q << endl;
    q.pop(); cout << q << endl;
    q.push(4); cout << q << endl;
    q.pop(); cout << q << endl;
    q.pop(); cout << q << endl;
    q.push(5); cout << q << endl;
    q.pop(); cout << q << endl;
    q.pop(); cout << q << endl;
    q.pop(); cout << q << endl;
}
```
<BR><BR>

__실행 결과__

<img src="https://user-images.githubusercontent.com/80774412/221362412-c00c05c9-75a8-4b42-bee2-d61c621c4a3d.PNG"></img>

프로그램을 실행시켜 큐가 잘 동작하는 모습을 확인했습니다.