2022.01

프로그래머스 > 코딩테스트 고득점 Kit > 깊이/너비 우선 탐색(DFS/BFS)

# 네트워크


## 문제설명

<img src="https://user-images.githubusercontent.com/80774412/206369839-c1fd6a64-8b19-45e9-91a7-5ca406e0cb9c.PNG" title="programmers_network"></img>

<img src="https://user-images.githubusercontent.com/80774412/206370015-2585cd6a-5d3b-4a4e-aa09-02836c6f1486.PNG" title="programmers_network"></img>

<img src="https://user-images.githubusercontent.com/80774412/206370062-4ce0e95e-1128-4a9d-8724-f07ab93d2bb1.PNG" title="programmers_network"></img>


<br><Br>

## 나의 풀이
---

```c++
#include <string>
#include <vector>
#include <queue>

using namespace std;

void dfs(vector<vector<int>> &computers, vector<bool> &visited, int idx)
{
    queue<int> q;
    q.push(idx);
    visited[idx] = true;
    
    while(!q.empty())
    {
        int cur = q.front();
        q.pop();
        
        for(int i = 0; i < computers.size(); i++)
        {
            if(!visited[i] && computers[cur][i] == 1)
            {
                q.push(i);
                visited[i] = true;
            }
        }
    }
}

int solution(int n, vector<vector<int>> computers) {
    int answer = 0;
    vector<bool> visited(n, false);

    for(int i = 0; i < n; i++)
    {
        if(visited[i] == false)
        {
            dfs(computers, visited, i);
            answer++;
        }
    }
    return answer;
}
```
우선 하나의 네트워크에 포함된 컴퓨터라는 조건이 있습니다.

하나의 컴퓨터를 시작 노드로 설정하고 연결된 컴퓨터들의 경로를 따라 검색해야 하므로 DFS 방법을 선택했습니다.

* 네트워크의 개수를 알기위해서는 우선 하나의 노드를 시작 노드로 설정했습니다.

* 큐에 시작 노드를 담고 방문여부 벡터의 값을 true로 바꿔 방문 체크를 했습니다.

* 시작노드와 연결된 즉, 시작 노드와 같은 네트워크에 있는 노드들을 검색하여 큐에 모두 담아주고 방문체크를 해줬습니다.

* 해당 네트워크와 연결된 노드들이 더 없는지 모두 검색하기 위해서 큐에 담아 놓은 노드들을 하나씩 꺼내 방문하지 않은 연결된 노드가 있다면 모두 큐에 담아주는 과정을 반복합니다.

* 큐에 담긴 모든 값이 탐색 종료되어 큐에 담긴 노드가 없다면 해당 네트워크에 연결된 모든 노드들을 검색한 시점이 됩니다.

* 그 이후에 존재하는 노드들 중 방문하지 않은 노드가 있다면, 또 다른 네트워크 연결망이 존재한다는 의미이므로 네트워크 개수 역할을 하는 answer의 값을 1 늘려주고,

    방문하지 않은 노드를 시작 노드로 하여 또다른 네트워크 연결망 탐색을 시작합니다.<br><Br>

