2023.06.10

# __[프로그래머스 LV2] 전력망 두개로 나누기__

----

## __문제__

[전력망 두개로 나누기](https://school.programmers.co.kr/learn/courses/30/lessons/86971)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <queue>

using namespace std;

struct NODE
{
    int start;
    int end;
    
    NODE(int _n, int _e){start = _n; end = _e;}
};

// 송전탑 개수, 전선정보
int solution(int n, vector<vector<int>> wires) {
    int answer = n;
    
    for(int i = 0; i < wires.size(); i++)
    {
        queue<NODE> q;
        q.push(NODE(wires[i][0], wires[i][1]));
        vector<bool> visited(wires.size());
        visited[i] = true;
        int index = 0;
        while(!q.empty())
        {
            NODE cur = q.front();
            q.pop();
            index++;
            
            for(int j= 0; j < wires.size(); j++)
            {
                if(visited[j] == true)
                    continue;
                
                q.push(NODE(wires[j][0], wires[j][1]));
                visited[j] = true;
            }
        }
       int diff = 0;
        if(index <= n - index)
           diff = n-index - index;
       else
           diff = index - (n - index);
                   
        answer = min(answer, diff);
    }
    
    return answer;
}
```

whires를 순회하면서 끊어진 부분을 바꿔줬습니다.

그리고 분리된 전력망 하나를 모두 bfs탐색하면서 노드의 개수를 구하여

전체 노드 개수에서 빼줘 다른 전력망의 개수도 구해줍니다.

답은 그 둘의 차 중 가장 작은 수를 구합니다.<br><Br>

## __수정 답안__

```c++
#include <string>
#include <vector>
#include <queue>
#include <map>
#include <iostream>

using namespace std;

// 송전탑 개수, 전선정보
int solution(int n, vector<vector<int>> wires) {
    int answer = n;
    
    map<int, vector<int>> nodes;
    // 1. 전선들을 맵 컨테이너에 매핑합니다.
    for(int i = 0; i < wires.size(); i++)
    {
        // 1-1. 양방향 검색할 수 있도록 매핑해줍니다.
        nodes[wires[i][0]].push_back(wires[i][1]);
        nodes[wires[i][1]].push_back(wires[i][0]);
    }
    
    // 2. 모든 노드를 하나씩 끊어줍니다.
    for(int i = 0; i < wires.size(); i++)
    {
        // 2-1. 끊어진 노드 설정
        vector<vector<bool>> obstacle(n+1, vector<bool>(n+1, true));
        obstacle[wires[i][0]][wires[i][1]] = false;
        obstacle[wires[i][1]][wires[i][0]] = false;
        
        // 2-2. 방문체크, 연결된 노드 큐
        vector<bool> visited(n+1);
        queue<int> q;
        visited[wires[i][0]] = true;
        q.push(wires[i][0]);
        
        // 2-3. 하나의 노드를 기준으로 BFS 탐색
        //      index를 사용하여 큐에 담겼던 노드의 개수를 체크합니다.
        int index = 0;
        while(!q.empty())
        {
            int cur = q.front();
            q.pop();
            index++;
            
            for(int j= 0; j < nodes[cur].size(); j++)
            {
                int nextNode = nodes[cur][j];
                
                if(obstacle[cur][nextNode] == false || visited[nextNode] == true)
                    continue;
                
                q.push(nextNode);
                visited[nextNode] = true;
            }
        }
        
        // 3. 각 전력망의 개수 차 계산
        //    무조건 전체 개수에서 반으로 나뉘기 때문에 전체 개수에서 차감합니다.
        int diff = 0;
        if(index <= n - index)
           diff = n-index - index;
        else
           diff = index - (n - index);
        
        // 가장 작은 값을 답으로 지정
        answer = min(answer, diff);
    }
    return answer;
}
```

큐에 NODE 구조체를 담는 방식에서 연결된 다음 노드만을 담는 방식으로 변경했습니다.

또한 모든 전력망 정보를 map에 매핑하여 BFS탐색 내부에서 다음 노드 검색횟수를 줄였습니다.

