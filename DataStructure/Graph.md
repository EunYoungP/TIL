2023.03.03

# __그래프 구현하기__

## 개요

- [그래프 구조](#그래프의-구조)

- [인접 행렬](#인접-행렬-adjacency-matrix)

- [인접 리스트](#인접-리스트-adjacency-list)

- [그래프 구현_인접 배열](#cpp-그래프-구현인접-배열)

- [그래프 구현_인접 리스트](#cpp-그래프-구현인접-리스트)




## __그래프의 구조__

<img src="https://user-images.githubusercontent.com/80774412/222712085-78362c90-26d6-4001-bd43-7a3265a0df63.png"></img>

그래프는 __정점 Vertex__ (또는 node) 와 __간선 Edge__ 로 이루어져 있습니다.

따라서 멤버로 __노드의 개수__ 와 간선의 데이터를 저장해주는 __인접 노드 정보__ 를 가지고 있습니다.<br><Br>

# __인접 노드 정보__

인접 노드에 대한 정보는 2차원 배열로 나타낸 __인접 행렬__ 과 연결리스트의 1차원 배열로 나타낸 __인접 리스트__ 로 나타낼 수 있습니다.<br><br>

## __인접 행렬 Adjacency matrix__

2차원 배열로 이루어집니다.

노드 i와 j가 간선으로 연결 유무에 따라서,

인접 행렬[i][j] 를 1/0 으로 표현합니다.

- 인접 행렬 장점

   - 두 노드의 간선 정보를 확인하는 것이 빠릅니다 O(1).

    - 새로운 간선의 추가, 삭제가 빠릅니다 O(1).

- 인접 행렬 단점

     - 배열의 크기가 노드의 개수의 제곱입니다. 

        따라서 간선의 개수가 적은 그래프의 경우 사용하는 데이터에 비해 메모리가 낭비될 수 있습니다.

    - 특정 노드에 인접한 노드를 찾기위해 모든 노드를 순회해야 합니다.

    - 노드를 추가/삭제 시간이 오래 걸립니다.O(N^2)

    - 그래프의 모든 간선수를 찾는 시간이 오래 걸립니다.O(N^2)

__밀집 그래프__ 에서 사용하는 것이 유용합니다.(i.e. 간선이 많은 그래프)
<BR><BR>

## __인접 리스트 Adjacency List__

연결리스트의 1차원 배열로 이루어집니다.

인접 리스트는 시작 노드를 기준으로 연결된 노드의 리스트를 1차원 배열로 저장합니다.

따라서 배열의 크기는 노드의 개수와 같습니다.

- 인접 리스트 장점

    - 메모리 효율이 좋습니다. 메모리 사용량이 노드의 수가 아닌 간선의 수에 따라 달라지기 때문입니다.

    - 노드의 추가/삭제 속도가 빠릅니다.O(1)

    - 인접한 노드를 검색하는 것이 빠릅니다.O(N+E)
    <Br><Br>

- 인접 리스트 단점

    - 두 노드의 간선 정보를 확인하기 위해 시간이 오래걸립니다.O(N^2)<br><br>

노드의 개수가 많고 간선의 개수가 적은 __희소 그래프__ 에  사용하는 것이 효율적입니다.<br><Br>

## __그래프의 종류__

여러가지 특징에 따라서 나뉘어 질 수 있습니다. (e.g. 방향성 유무, 가중치 유무, out-in )

| 희소 그래프 --|-- 밀집 그래프 |

| 방향 그래프 --|-- 무방향 그래프 |

가중치 그래프

멀티그래프


## __CPP 그래프 구현(인접 배열)__ 

```c++
#include <iostream>
using namespace std;

#define MAX_VTX 256

// 배열을 이용한 그래프 구현
class ArrayAdjGraph
{
public:
	int size;
	char vertexes[MAX_VTX];
	int adjacency[MAX_VTX][MAX_VTX];

public:
	ArrayAdjGraph() :size(0) {}
	~ArrayAdjGraph() {}

	char getVertex(int i)
	{
		return vertexes[i];
	}

	int getEdge(int i, int j)
	{
		return adjacency[i][j];
	}

	void setEdge(int i, int j, int value)
	{
		adjacency[i][j] = value;
	}

	void InsertVertex(char v)
	{
		if (IsFull())return;

		vertexes[size++] = v;
	}

    // 미리 할당된 배열 인덱스에 값을 넣어줍니다.
	void InsertEdge(int v1, int v2)
	{
		setEdge(v1, v2, 1);
		setEdge(v2, v1, 1);
	}

	bool IsEmpty()
	{
		return size <= 0;
	}

	bool IsFull()
	{
		return size >= MAX_VTX;
	}
};

ostream& operator <<(ostream& out, ArrayAdjGraph& graph)
{
	out << "그래프 ";
	for (int i = 0; i < graph.size; i++)
	{
		out << graph.getVertex(i) << " [";
		for (int j = 0; j < graph.size; j++)
		{
			out << " " << graph.getEdge(i, j);
		}
	}
	out << endl;
}

int main()
{
	ArrayAdjGraph arrgraph;

	arrgraph.InsertVertex('A');
	arrgraph.InsertVertex('B');
	arrgraph.InsertVertex('C');

	arrgraph.InsertEdge(0, 1);
	arrgraph.InsertEdge(0, 2);
	arrgraph.InsertEdge(1, 2);

	cout << arrgraph << endl;
}
```

인접 배열에 미리 메모리가 할당되어 인접 노드 정보가 추가될 때,

배열의 인덱스에 해당하는 메모리에 값을 넣어주기만 하면 됩니다.

노드가 추가될 때에도 연결리스트로 구현했을 때와는 달리, 인접 노드 정보의 크기 갱신이 필요없습니다.

인접 리스트의 경우 노드의 개수가 배열의 크기이기 때문에 노드를 추가하면 인접 리스트의 크기를 늘려줘야합니다.

하지만 2차원 배열로 구현할 경우 최대 노드의 크기만큼 미리 배열을 초기화 했기 때문에 노드 추가 시에도 갱신해줄 필요가 없습니다.

## __CPP 그래프 구현(인접 리스트)__ 

```C++
#include <iostream>
using namespace std;

#define MAX_VTXS 256

// 인접 리스트의 연결리스트를 구현할 노드입니다.
class Node
{
public:
    int id;
    Node* next;

    Node(int _id, Node* _node) { id = _id; next = _node; }
    ~Node(){}
};

class Graph
{
public:
    int size;                   // 정점의 개수
    char vertexes[MAX_VTXS];    // 정점의 이름
    Node* adjacency[MAX_VTXS];  // 인접 리스트

    Graph() :size(0){}
    ~Graph(){}

public:
    void InsertVertex(char name)
    {
        if (IsFull()) return;

        vertexes[size] = name;      // 정점을 추가
        adjacency[size++] = NULL;   // 추가된 정점의 인덱스를 인접리스트에 추가
    }

    void InsertEdge(int u, int v)
    {
        adjacency[u] = new Node(v, adjacency[u]);
        adjacency[v] = new Node(u, adjacency[v]);
        // 방향 그래프 구현 시, u -> v 간선만 추가됩니다.
    }

    // v번째 정점의 인접 리스트를 반환합니다.
    Node* GetAdjacency(int v)
    {
        return adjacency[v];
    }

    bool IsFull()
    {
        return size >= MAX_VTXS;
    }
};

ostream& operator <<(ostream& out, Graph& graph)
{
    for (int i = 0; i < graph.size; i++)
    {
        Node* temp = graph.adjacency[i];
        out << graph.vertexes[temp->id] << " 정점 [";
        while (temp != NULL)
        {
            out << " " <<  temp->id;
            temp = temp->next;
        }
        out << "] " << endl;
    }
    return out;
}

int main()
{
    Graph graph;
    
    graph.InsertVertex('A');
    graph.InsertVertex('B');
    graph.InsertVertex('C');

    graph.InsertEdge(0, 1);
    graph.InsertEdge(0, 2);
    graph.InsertEdge(1, 2);

    cout << graph << endl;
}
```
인접 리스트는 메모리를 미리 할당하지 않고,

노드나 간선이 추가될 떄 할당됩니다. 

따라서 메모리의 낭비를 줄일 수 있습니다.

<Br><Br>
