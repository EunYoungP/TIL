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
    
    // dfs
    BFS(edges, visited, dist, 1, 0);
    

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

bfs 참고 풀이입니다.