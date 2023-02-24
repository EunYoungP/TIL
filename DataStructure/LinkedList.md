2023.02.22

# __링크드 리스트 구현하기__

__리스트__ 라는 순서를 가진 자료구조에는 __배열__ 과 __링크드리스트__ 가 존재합니다.

배열은 원소의 검색에 시간 복잡도가 짧고, 링크드 리스트는 원소의 추가, 삭제 등의 작업에서 시간 복잡도의 장점을 가집니다.

링크드 리스트에서 특정 인덱스 K 의 값을 __검색__ 하려면 첫 번째 노드에서부터 다음 노드 포인터를 따라서 검색해야 하기때문에 __O(K)__ 의 시간 복잡도를 가집니다.

하지만 값을 __삽입__ 하는 경우에는 K의 이전노드의 다음노드를 추가할 노드로 바꿔주고, 추가할 노드의 다음노드를 그다음 노드로 설정해주면 되기떄문에 __O(1)__ 의 시간복잡도를 가집니다.

K 인덱스의 원소 __삭제__ 의 경우에는 K 의 이전 노드가 가진 다음노드의 포인터를 K 의 다음노드를 가리키게 함으로써 또한 __O(1)__ 의 시간 복잡도로 값을 삭제할 수 있습니다.

따라서 링크드 리스트는 삽입, 삭제를 많이 사용해야하는 경우 유용하게 쓰일 수 있습니다.
<Br><br>

## __연결리스트 구조__ 

<img src="https://user-images.githubusercontent.com/80774412/220640390-bb22b2d9-1be1-4f97-85a6-e471574ffe21.png"></img>

각 원소 노드는 자신의 다음 노드를 포인터로 가리키고있습니다.

각 원소 노드 = 자신이 보존중인 값 + 다음 노드를 가리키는 포인터

첫 번째 값을 가지고 있는 노드를 __head node__ 라고 합니다.

마지막 노드는 아무것도 가리키지 않습니다.(null)<BR><BR>


```c++
using namespace std;
#include <iostream>

// 예외 처리용 클래스
class InvalidIndexException {};

// 각 노드를 표현하는 클래스
template<typename T>
class ListNode
{
public:
    // 노드가 가지고있는 값
	T value;
    // 다음 노드를 가리키는 포인터
	ListNode<T>* next;

	ListNode<T>() : next(nullptr){}
	ListNode<T>(T _value, ListNode<T>* _next) : value(_value), next(_next){}
};

// 링크드 리스트를 표현하는 클래스
template<typename T>
class LinkedList
{
public:
    // 노드의 총 개수
	int size;
    // 첫 번째 값을 가진 헤드노드
	ListNode<T>* head;

	LinkedList<T>() : size(0), head(nullptr){}

	~LinkedList<T>()
	{
		ListNode<T>* node1 = head, *node2;

		if (node1 != nullptr)
		{
			node2 = node1->next;
			delete node1;
			node1 = node2;
		}
	}

	// k 인덱스에 value 값을 넣습니다.
	void insert(int k, T value)
	{
		try
		{
			if (k < 0 || k > size)throw InvalidIndexException();

			if (k == 0)
			{
				head = new ListNode<T>(value, head);
			}
			else
			{
				ListNode<T>* dest = head;
				for (int i = 0; i < k - 1; i++)
				{
					dest = dest->next;
				}
				dest->next = new ListNode<T>(value, dest->next);
			}
			size++;
		}
		catch(InvalidIndexException e)
		{
			cout << "잘못된 인덱스입니다." << endl;
			exit(1);
		}
	}

    // k 인덱스의 값을 삭제합니다.
	void erase(int k)
	{
		try
		{
			if (k < 0 || k > size)throw InvalidIndexException();

			if (k == 0)
			{
				head = head->next;
			}
			else
			{
				ListNode<T>* dest = head, *temp;
				for (int i = 0; i < k-1; i++)
				{
					dest = dest->next;
				}
				temp = dest->next->next;
				dest->next = temp;
			}
			size--;
		}
		catch (InvalidIndexException e)
		{
			cout << "잘못된 인덱스입니다." << endl;
			exit(2);
		}
	}
};

// 스택의 값을 top -> bottom 순으로 나열해서 출력하는 기능입니다.
// iostream 의 클래스 stream 의 연산자 << 오버라이딩 
template<typename T>
ostream& operator <<(ostream& out, LinkedList<T>& linkedList)
{
    ListNode<T>* temp = linkedList.head;
    out << "[";
    for (int i = 0; i < linkedList.size; i++)
    {
        out << temp->value;
        temp = temp->next;
        if (i < linkedList.size - 1)out << ", ";
    }
    out << "]";
    return out;
}

int main()
{
    LinkedList<int> S;
    S.insert(0); cout << S << endl;
    S.insert(1); cout << S << endl;
    S.insert(2); cout << S << endl;
    S.insert(3); cout << S << endl;
    S.erase(); cout << S << endl;
    S.insert(4); cout << S << endl;
    S.erase(); cout << S << endl;
    S.erase(); cout << S << endl;
    S.insert(5); cout << S << endl;
    S.erase(); cout << S << endl;
    S.erase(); cout << S << endl;
    S.erase(); cout << S << endl;
}
```


----
[참고자료](https://blog.naver.com/kks227/220781402507)