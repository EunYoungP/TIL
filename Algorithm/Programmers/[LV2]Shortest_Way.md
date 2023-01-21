```c++
#include<vector>
#include<queue>
using namespace std;


 ## __오답__

void bfs(vector<vector<int>>& maps,vector<vector<bool>>& visited, int x, int y, int& answer)
{
    if(x == maps[0].size() && y == maps.size())
    {
        return;
    }
    answer++;

    visited[x][y] = true;
    vector<int> xDir = {1,0,-1,0};
    vector<int> yDir = {0,-1,0,1};
    for(int i = 0; i < 4; i ++)
    {
        int newX = x + xDir[i];
        int newY = y + yDir[i];

        if(newX >= maps[i].size()|| newX < 0 || newY >= maps.size() || newY < 0)
            continue;
        if(maps[newX][newY] == 1 && !visited[newX][newY])
            bfs(maps, visited, newX, newY, answer);
    }
}

int solution(vector<vector<int>> maps)
{
    int answer = 0;
    vector<vector<bool>> visited;

    bfs(maps, visited, 0, 0, answer);

    return answer;
}
```

BFS와 DFS의 개념이 확립되지 않아 BFS인 내용을 DFS로 기입하고 재귀함수를 섞어서 썼습니다.

BFS는 너비 우선 탐색, DFS는 깊이 우선 탐색입니다.

BFS는 큐를 사용하여 자연스럽게 출발지로부터 거리 순으로 방문하기 때문에 최단 거리를 구하는데 사용됩니다.

DFS는 깊이 우선으로 탐색하기 때문에 입력 크기가 작을때 유리하고, 가장 먼저 구해진 방법이 최단 거리는 아닐 수 있습니다.

위 문제에서는 최단 거리를 구하는 문제이기 때문에 BFS를 사용해야합니다.

## __수정답안__

```C++
#include<vector>
#include<queue>
#include<iostream>
using namespace std;

struct POS
{
    int x, y;
};

int solution(vector<vector<int>> maps)
{
    int answer = -1;
    
    int x = maps[0].size();
    int y = maps.size();
    
    // 우, 하, 좌, 상
    vector<int> xDir = {1,0,-1,0};
    vector<int> yDir = {0,1,0,-1};
        
    vector<vector<bool>> visited;
    queue<POS> q;
    
    q.push({0, 0});
    int index = -1;
    while(!q.empty())
    {
        POS cur = q.front();
        cout <<"q 값 : " << cur.x << " "<< cur.y << endl;
        q.pop();
        
        if(cur.x == x-1 && cur.y ==y-1)
            return index;
        
        for(int i = 0; i < 4; i ++)
        {
            index++;
            cout<< index << endl;
            
            int newX = x + xDir[i];
            int newY = y + yDir[i];

            if(newX >= maps[0].size()-1|| newX < 0 || newY >= maps.size()-1 || newY < 0)
                continue;
            if(maps[newY][newX] == 0)
                continue;
            if(visited[newY][newX])
                continue;
            
            q.push({newY, newX});
        }
    }
    return answer;
}
```

