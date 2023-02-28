2023.01
2023.02.03
2023.02.13

# __[프로그래머스 LV4] 가장짧은거리__

BFS/DFS
---- 
<BR>

## __오답__
```c++
#include<vector>
#include<queue>
using namespace std;
 
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
    
    POS(int _x, int _y){x = _x; y = _y;}
};

int solution(vector<vector<int>> maps)
{
    int answer = -1;
    
    int x = maps[0].size();
    int y = maps.size();
    
    // 우, 하, 좌, 상
    vector<int> xDir = {1,0,-1,0};
    vector<int> yDir = {0,1,0,-1};
        
    vector<vector<bool>> visited(y,vector<bool>(x));
    vector<vector<int>> dist(y,vector<int>(x));
    queue<POS> q;
    
    q.push(POS(0,0));
    visited[0][0] = true;
    dist[0][0] = 1;
    
    while(!q.empty())
    {
        POS cur = q.front();
        q.pop();
        
        int curX = cur.x;
        int curY = cur.y;
        
        for(int i = 0; i < 4; i ++) 
        {
            int newX = curX + xDir[i];
            int newY = curY + yDir[i];

            if(newX >= x|| newX < 0 || newY >= y || newY < 0)
            {
                continue;
            }
                
            if(maps[newY][newX] == 0)
            {
                continue;
            }
                
            if(visited[newY][newX])
            {
                continue;
            }
            
            q.push(POS(newX, newY));
            visited[newY][newX] = true;
            dist[newY][newX] = dist[curY][curX] + 1;
        }
    }
    
    if(!visited[y-1][x-1])
        return -1;
    else
        return dist[y-1][x-1];
}
```


## __22.02.03__

두번째 복습

```c#
#include<vector>
#include<queue>
using namespace std;

struct POS
{
    int x, y, dist;
    
    POS(int _x, int _y, int _dist){x = _x; y = _y; dist = _dist;}
};

int solution(vector<vector<int>> maps)
{
    int answer = -1;
    
    int x = maps[0].size();
    int y = maps.size();
    
    // 우, 하, 좌, 상
    vector<int> xDir = {1,0,-1,0};
    vector<int> yDir = {0,1,0,-1};
        
    vector<vector<bool>> visited(y,vector<bool>(x));
    queue<POS> q;
    
    q.push(POS(0,0,1));
    visited[0][0] = true;
    
    while(!q.empty())
    {
        POS cur = q.front();
        q.pop();
        
        int curX = cur.x;
        int curY = cur.y;
        
        if(curX == x-1 && curY == y-1)
            return cur.dist;
        
        for(int i = 0; i < 4; i ++) 
        {
            int newX = curX + xDir[i];
            int newY = curY + yDir[i];

            if(newX >= x|| newX < 0 || newY >= y || newY < 0)
            {
                continue;
            }
                
            if(maps[newY][newX] == 0)
            {
                continue;
            }
                
            if(visited[newY][newX])
            {
                continue;
            }
            
            q.push(POS(newX, newY, cur.dist+1));
            visited[newY][newX] = true;
        }
    }
    return -1;
}

```
두번째 풀이에서는 첫번째에서 이중벡터로 구현했던 거리에대한 정보를 구조체의 멤버로 넣었습니다.

dist 벡터의 인덱스 위치에 거리값을 저장하지 않고 while문에서 검사 조건을 통과하여 큐에 추가되는 POS 값에 거리값을 증가시키며

while문이 끝나기 전 모든 x, y의 값이 목적지의 인덱스와 같아지면 해당 POS 구조체의 dist값을 반환하도록 구현했습니다.


## __22.02.13__

세 번째 복습

```c++
#include<vector>
#include<iostream>
using namespace std;

int offsetX[] = {1,0,-1,0};
int offsetY[] = {0,1,0,-1};

// struct POS
// {
//     int x, y, dist;
//     POS(int _x, int _y, int _dist){x = _x; y = _y; dist = _dist;}
// };

void DFS(vector<vector<int>> maps, vector<vector<int>>& distmap, vector<vector<bool>>& visited, int x, int y, int index)
{
    if(visited[maps.size()-1][maps[0].size()-1] == true)
    {
        return;
    }
    
    for(int i= 0; i < 4; i++)
    {
        int nx = x + offsetX[i];
        int ny = y + offsetY[i];
        
        if(nx < 0 || nx >= maps[0].size() || ny < 0 || ny >= maps.size())
            continue;
        
        if(visited[ny][nx] == true)
            continue;
        
        if(maps[ny][nx] == 0)
            continue;
        
        visited[ny][nx] = true;
        distmap[ny][nx] = index+1;
        DFS(maps, distmap, visited, nx, ny, index+1);
    }
    return;
}

int solution(vector<vector<int>> maps)
{
    int mapX = maps[0].size();
    int mapY = maps.size();
    
    vector<vector<bool>> visited(mapY, vector<bool>(mapX));
    vector<vector<int>> distmap(mapY, vector<int>(mapX));
    
    visited[0][0] = true;
    distmap[0][0] = 1;
    DFS(maps, distmap, visited, 0, 0, 1);
    
    if(visited[maps.size()-1][maps[0].size()-1] == true)
        return distmap[maps.size()-1][maps[0].size()-1];
    else 
        return -1;
}
```

처음으로 dfs를 사용하여 문제를 풀어봤습니다.

대부분의 테스트케이스는 통과가되었는데 2개의 테스트가 통과되지 않았고

효율성 테스트에서 모두 시간초과가 나왔습니다.

dfs는 모든 노드를 탐색하여 거리값을 비교하고, bfs는 모든 값을 탐색하지 않고 인접노드 순서대로 탐색하여 목적지까지의 거리를 구하기때문에

효율성에서 차이가 있었습니다.

따라서 이런 최단 거리를 구하는 문제는 dfs를 이용하여 풀 수도 있지만, 효율성에서 문제가 있다면 bfs를 이용하여 풀이하는것이 효율적이라고 볼 수 있습니다.<br><Br>

## __22.02.28__

네 번째 복습

```c++
#include<vector>
#include<queue>
#include<iostream>
using namespace std;

// 검정 벽 못감
// 맵 벗어난 곳 못감
struct Block
{
    int x, y, dist;
    
    Block(int _x, int _y, int _dist){x = _x; y = _y; dist = _dist;}
};

int solution(vector<vector<int> > maps)
{
    int answer = -1;
    
    int mapX = maps[0].size();
    int mapY = maps.size();
    
    // 상 하 좌 우
    int offsetX[] = {0, 0, -1, 1};
    int offsetY[] = {-1, 1, 0, 0};
    
    queue<Block> q;
    vector<vector<bool>> visited(mapY,vector<bool>(mapX));
    vector<vector<int>> dist(mapY,vector<int>(mapX));
    
    q.push(Block(0,0,1));
    visited[0][0] = true;
    dist[0][0] = 1;
    
    while(!q.empty())
    {
        Block curBlock = q.front();
        int curX = curBlock.x;
        int curY = curBlock.y;
        q.pop();
        
        if(curX == mapX-1 && curY == mapY-1)
            return curBlock.dist;
        
        for(int i = 0; i < 4; i++)
        {
            int newX = curX + offsetX[i];
            int newY = curY + offsetY[i];
            
            // 1. 조건문: 인덱스가 범위를 벗어나는지 검사
            if(newY < 0 || newX < 0 || newY >= mapY || newX >= mapX)
                continue;
            // 2. 방문했던 블럭인지 검사
            if(visited[newY][newX] == true)
                continue;
            // 3. 막혀있는 블럭인지 검사
            if(maps[newY][newX] == 0)
                continue;
            
            q.push(Block(newX, newY, curBlock.dist+1));
            visited[newY][newX] = true;
            dist[newY][newX] = curBlock.dist + 1;
        }
    }
    
    if(visited[mapY-1][mapX-1] == true)
    {
        answer = dist[mapY-1][mapX-1];
    }
    return answer;
}
```

풀이 과정 중 있던 실수 하나는,

for 문 안에 해당 블럭을 큐에 넣어도 되는 블럭인지 검사하는 조건문들 이 세개 있습니다.

조건문 세개 중 인덱스 검사하는 조건문을 가장 먼저 실행해주지 않으면,

segmentation fault 오류가 발생합니다.

잘못된 인덱스가 2, 3번 조건문에서 검사에 사용될 수 있기 떄문입니다.

따라서 항상 인덱스 검사를 가장 먼저 실행 해줘야합니다.
