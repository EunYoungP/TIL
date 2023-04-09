2023.04.01

# __[프로그래머스 LV3] 가장 먼 노드__



## __문제__

[프로그래머스_가장 먼 노드](https://school.programmers.co.kr/learn/courses/30/lessons/49189)<br><Br>


## __나의 풀이__(오답)
```c++
#include <string>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

void DFS(unordered_set<int, vector<int>> edges, vector<int> visited,vector<int>& dist, int startIndex, int dist)
{
    for(int i = 0; i <edges[startIndex].size(); i++)
    {
        int nextIndex = edges[startIndex][i];
        
        if(visited[nextIndex])continue;
        
        dist[nextIndex] = dist++;
        visited[nextIndex] = ture;
        DFS(edges, visited, dist, nextIndex, dist++);
    }
    return;
}

int solution(int n, vector<vector<int>> edge) {
    int answer = 0;
    int minDist = n+1;
    vector<int> dist(n+1);
    vector<bool> visited(n+1);
    
    vector<vector<int>> edges(n+1, vector<int>(n+1));
    queue<int> q;
    
    // 2차원 벡터를 map으로 변경
    for(int i= 0; i < edge[0].size(); i++)
    {
        edges[edge[i][0]].push_back(edge[i][1]);
    }

    // bfs
    for(int i = 2; i <= n; i++)
    {
        vector<bool> visited(n+1);
        q.push(i);
        
        while(!q.empty())
        {
            int cur = q.front();
            q.pop();
            
            if(cur == 1)
            {
                minDist = min(minDist, dist[i]);
                break;
            }
            for(int j = 0; j <edges[i].size(); j++)
            {
                int nextIndex = edges[i][j];

                if(visited[nextIndex])continue;

                dist[i]++;
                q.push(nextIndex);
            }
        }
    }

    for(int i = 0 ; i < dist.size(); i ++)
    {
        if(dist[i] == minDist)
            answer++;
    }
    
    return answer;
}
```

처음 방법으로 BFS를 이용하여 모든 노드의 요소에서 1에 도달할 때까지의 거리를 구한 뒤 값을 저장했습니다.

이 방법으로 모든 테스트는 시간 초과의 결과를 냈습니다.

따라서 DFS를 이용하여 모든 dist 벡터에 최소 값을 넣어 비교해주는 방식으로 변경했습니다.<br><Br>

## __나의 풀이 DFS__

```C++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

void DFS(vector<vector<int>> edges, vector<bool> visited, vector<int>& dist, int startIndex, int index)
{
    for(int i = 0; i < edges[startIndex].size(); i++)
    {
        int nextIndex = edges[startIndex][i];
        
        if(visited[nextIndex])
            continue;
        if(dist[nextIndex] <= index)
            continue;
        
        dist[nextIndex] = index;
        visited[nextIndex] = true;
        
        DFS(edges, visited, dist, nextIndex, index+1);

        // 반복문이 끝나지 않았다면 함수에서 전달받은 visited를 그대로 사용해야합니다.
        visited[nextIndex] = false;
    }
}

int solution(int n, vector<vector<int>> edge) {
    int answer = 0;
    int maxDist = 0;
    vector<int> dist(n+1, 20000);
    vector<bool> visited(n+1, false);
    vector<vector<int>> edges(n+1);
    
    // edge 안에 연결된 노드두개의 묶음 리스트가 들어가 있기 때문에
    // edge[0].size() 는 두개입니다.
    for(int i= 0; i < edge.size(); i++)
    {
        edges[edge[i][0]].push_back(edge[i][1]);
        // 양방향 이동이 가능하기 때문에 간선의 양 노드인덱스에 
        // 서로를 등록해줍니다.
        edges[edge[i][1]].push_back(edge[i][0]);
    }
    
    visited[1] = true;
    // dfs
    DFS(edges, visited, dist, 1, 1);

    for(int i = 2 ; i < dist.size(); i ++)
    {
        if(dist[i] > maxDist)
        {
            maxDist = dist[i];
            answer = 0;
            answer++;
        }
        else if(dist[i] == maxDist)
        {
            answer++;
        }
        else if(dist[i] < maxDist)
        {
            continue;
        }
    }
    return answer;
}
```

DFS 풀이로 간단한 테스트들은 모두 통과했습니다.

하지만 일부 테스트들에서 시간 초과가 일어났습니다.

보통  DFS는 모든 노드를 검색하여 BFS보다 시간 비용이 크기때문에 다시 BFS 풀이를
참고하여 풀이했습니다.<BR><bR>

## __참고 풀이__
```c++
#include <string>
#include <vector>
#include <queue>
#include <iostream>

using namespace std;

bool check[20001];

int solution(int n, vector<vector<int>> edge) {
    int answer = 0;
    int MAX = 0;
    vector<vector<int>> v(n + 1);
    queue<pair<int, int>> q;
    
    for(size_t i = 0; i < edge.size(); i++){
        int n1 = edge[i][0]; int n2 = edge[i][1];
        v[n1].emplace_back(n2);
        v[n2].emplace_back(n1);
    }
    
    q.emplace(1, 1);
    check[1] = true;
    
    while(!q.empty()){
        int num = q.front().first;
        int count = q.front().second;
        q.pop();

        for(size_t i = 0; i < v[num].size(); i++){
            if(check[v[num][i]] == false){
                check[v[num][i]] = true;
                q.emplace(v[num][i], count + 1);
                cout << v[num][i] << " " << count + 1 << endl;;
                if(MAX < count + 1){
                    answer = 1;
                    MAX = count + 1;
                }else if(MAX == count + 1) answer++;
            }
        }
    }
    return answer;
}
```
