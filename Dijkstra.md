# __다익스트라 알고리즘 Dijkstra__<br><br>

다익스트라 알고리즘은 다이나믹 프로그래밍을 활용한 대표적인 __최단 경로 탐색 알고리즘__ 입니다.

다익스트라 알고리즘이 다이나믹 프로그래밍 문제인 이유는 '최단 거리는 여러개의 최단 거리로 이루어져 있기' 때문입니다. 작은 문제가 큰 문제의 부분 집합에 속해있다고 볼 수 있는 __최적 구분 구조__ 입니다.

하나의 최단 거리를 구할 때 그 이전까지 구했던 최단 거리 정보를 그대로 사용한다는 특징이 있습니다.

__작동 과정__

1. 출발 노드를 설정합니다.

2. 출발 노드를 기준으로 각 노드의 최소 비용을 저장합니다.

3. 방문하지 않는 노드 중에서 가장 비용이 적은 노드를 선택합니다.

4. 해당 노드를 거쳐서 특정한 노드로 가는 경우를 고려하여 최소 비용을 갱신합니다.

5. 위 과정에서 3번 ~ 4번을 반복합니다.<br><br>


## __선형 탐색 다익스트라__

```c++
int number = 6;
int INF = 1_000_000;

int a[6][6] = {
     {0,2,5,1,INF,INF},
     {2,0,3,2,INF,INF},
     {5,3,0,3,1,5},
     {1,2,3,0,1,INF},
     {INF,INF,1,1,0,2},
     {INF,INF,5,INT,2,0},
};

bool visited[6];
int distance[6];
   
// 최소 거리를 가지는 노드를 반환합니다.
int getSmallIndex()
{
    int min = INF;
    int index = 0;

    // 거리비용 배열을 순회하며 가장 적은 비용값을 가진 노드 인덱스를 반환합니다.
    for(int i = 0; i < number; i++)
    {
        if(distance[i] < min && !visited[i])
        {
            min = distance[i];
            index = i;
        }
    }
    return index;
}

void dijkstra(int start)
{
    for(int i = 0; i < number; i++)
    {
        // 시작 노드에서 특정노드로 이동하는 거리의 비용입니다. 
        distance[i] = a[start][i];
    }

    // 방문한 시작 노드는 담아줍니다.
    visited[start] = true;
    int current;
    for(int i = 0; i < number -2; i++)
    {
        current = getSmallIndex();
        visited[current] = true;

        for(int j = 0; j < number; j++)
        {
            // 아직 방문하지 않은 노드들만 검사합니다.
            if(!visited[j])
            {
                if(distance[j] > distance[current] + a[current][j])
                {
                    distance[j] = distance[current] + a[current][j];
                }
            }
        }
    }
}
```
위 알고리즘은 최소 비용을 단순히 __선형 탐색__ 으로 찾습니다.

이 경우 다익스트라의 시간 복잡도가 O(N^2)으로 형성됩니다.

빠르게 작동시켜야 하는 경우 __힙 구조__ 를 활용하여 시간 복잡도를 O(NlogN) 으로 만들 수 있습니다.

## __인접리스트 방식 다익스트라__

```c++
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

int number = 6;
int infinit = 1000000;

// 간선 정보
vector<pair<int, int>> edges[7];
// 최소 비용
int distances[7];

void dijkstra(int start)
{
    // 기본적으로 연결되지 않은 경우 거리 비용은 무한입니다.
    distances[start] = 0;
    priority_queue<pair<int, int>> pq;     // 힙 구조
    pq.push(make_pair(start, 0));

    while(!pq.empty())
    {
        int cur = pq.top().first;

        int distance = = -pq.top().second;
        pq.pop();

        if(distances[cur] < )
    }
}

int main()
{
    // 기본적으로 연결되지 않은 경우 거리 비용은 무한입니다.
    for(int i = 0; i <= number; i++)
    {
        distances[i] = infinit;
    }

    edge[1].push_back(make_pair(2,2));
    edge[1].push_back(make_pair(3,5));
    edge[1].push_back(make_pair(4,1));

    edge[2].push_back(make_pair(1,2));
    edge[2].push_back(make_pair(3,3));
    edge[2].push_back(make_pair(4,2));

    edge[3].push_back(make_pair(1,5));
    edge[3].push_back(make_pair(2,3));
    edge[3].push_back(make_pair(4,3));
    edge[3].push_back(make_pair(5,1));
    edge[3].push_back(make_pair(6,5));

    edge[4].push_back(make_pair(1,1));
    edge[4].push_back(make_pair(2,2));
    edge[4].push_back(make_pair(3,3));
    edge[4].push_back(make_pair(5,1));

    edge[5].push_back(make_pair(3,1));
    edge[5].push_back(make_pair(4,1));
    edge[5].push_back(make_pair(6,2));

    edge[6].push_back(make_pair(3,5));
    edge[6].push_back(make_pair(5,2));

    dijkstra(1);
}

```