2023.02.23

# __스택 구현하기__

스택은 LIFO 자료구조입니다.

스택은 맨 위의 값만 다룰 경우에 사용합니다.

스택은 특정 방향에서만 삽입, 삭제 연산이 이루어지고,

push, pop, top 연산이 모두 시간 복잡도 O(1) 을 가지기 위해서 __연결 리스트__ 로 구현해야 합니다.<br><Br>


## __스택의 구조__

<img src="https://user-images.githubusercontent.com/80774412/220921539-157760c8-5f10-42a4-a03a-e0074c0cb9cc.png"></img>

만약 스택이 비어있어 원소가 하나도 없을 때 pop 이나 top 연산을 하면 __언더플로우__ 와 같은 문제가 발생합니다.

__push__ 연산 시, 연결 리스트의 tail 포인터가 top 원소를 가리키는 형태로 구현합니다. 새로운 원소를 만들어서 tail 로 지정합니다.

__pop__ 연산 시, tail 포인터가 가리키는 top 원소를 임시 노드 포인터로 가리키고 top 원소는 삭제합니다. 그리고 임시 노드가 가리키는 원소를 tail 원소로 지정합니다.

__top__ 연산은 tail 포인터가 가리키는 원소를 반환합니다.<br><Br>


## __스택 CPP 코드 구현__

```c++
#include <iostream>
using namespace std;

// 언더플로우 예외를 처리하기위한 클래스입니다.
class UnderflowException {};

template<typename T>
class ListNode
{
public:
    // 노드가 가지고 있는 값
    int value;
    // 노드가 가리키는 다음 원소
    ListNode<T>* next;

    ListNode<T>() : next(nullptr) {}
    ListNode<T>(int _value, ListNode<T>* _next) : value(_value), next(_next) {}
};

template<typename T>
class Stack
{
public:
    // 스택 원소의 총 개수
    int size;
    // 스택의 가장 최신 원소
    ListNode<T>* tail;

    Stack<T>(): size(0), tail(nullptr){}

    ~Stack<T>()
    {
        ListNode<T>* t1 = tail, * temp;

        temp = t1;
        delete(t1);
        t1 = temp->next;
    }

    // 노드를 추가합니다.
    void push(int value)
    {
        // 꼬리 노드를 새로 생성한 노드로 변경합니다.
        tail = new ListNode<T>(value, tail);
        size++;
    }

    // 스택의 가장 최신 노드를 삭제합니다.
    void pop()
    {
        try
        {
            if (size == 0)throw UnderflowException();

            ListNode<T>* temp = tail->next;
            delete(tail);
            tail = temp;
            size--;
        }
        catch (UnderflowException e)
        {
            cout << "비어있는 스택에 pop을 시도했습니다." << endl;
            exit(1);
        }
    }

    // 스택의 가장 최신 노드를 반환합니다.
    T top()
    {
        try
        {
            if (size == 0)throw UnderflowException();

            return tail->value;
        }
        catch (UnderflowException e)
        {
            cout << "비어있는 스택에 top을 시도했습니다." << endl;
            exit(2);
        }
    }
};

// 스택의 값을 top -> bottom 순으로 나열해서 출력하는 기능입니다.
// iostream 의 클래스 stream 의 연산자 << 오버라이딩 
template<typename T>
ostream& operator <<(ostream& out, Stack<T>& stack)
{
    ListNode<T>* temp = stack.tail;
    out << "top [";
    for (int i = 0; i < stack.size; i++)
    {
        out << temp->value;
        temp = temp->next;
        if (i < stack.size - 1)out << ", ";
    }
    out << "] bottom";
    return out;
}

int main()
{
    Stack<int> S;
    S.push(0); cout << S << endl;
    S.push(1); cout << S << endl;
    S.push(2); cout << S << endl;
    S.push(3); cout << S << endl;
    S.pop(); cout << S << endl;
    S.push(4); cout << S << endl;
    S.pop(); cout << S << endl;
    S.pop(); cout << S << endl;
    S.push(5); cout << S << endl;
    S.pop(); cout << S << endl;
    S.pop(); cout << S << endl;
    S.pop(); cout << S << endl;
}
```

__실행 결과__

<img src="https://user-images.githubusercontent.com/80774412/220922549-c73a9f16-d359-458b-ba7f-bbbf362f53f6.PNG"></img>

실행 결과 추가, 삭제 가 잘 작동하는 것을 알 수 있습니다.