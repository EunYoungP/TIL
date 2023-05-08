2023.05.05

# __[프로그래머스 LV2] 미로 탈출__

BFS

## __문제__

[미로 탈출](https://school.programmers.co.kr/learn/courses/30/lessons/159993)<br><Br>

## __나의 풀이__(오답)
```c++
#include <string>
#include <vector>
#include <queue>
#include <iostream>

using namespace std;

struct POS
{
    int x;
    int y;
    int dist;
    POS(int _x, int _y, int _dist){x = _x; y = _y; dist = _dist;}
};

int solution(vector<string> maps) {
    // 미로 탈출 최소 시간
    int answer = 0;
    
    vector<vector<char>> mapV(maps.size(), vector<char>(maps[0].size()));
    vector<vector<int>> dist(maps.size(), vector<int>(maps[0].size()));
    vector<vector<bool>> visited(maps.size(), vector<bool>(maps[0].size()));
    
    // 상 우 하 좌
    int offsetX[] = {0, 1, 0, -1};
    int offsetY[] = {-1, 0, 1, 0};
    
    queue<POS> q;
    POS startPos = POS(0,0,0);
    POS exitPos = POS(0,0,0);
    bool isLevered = false;
    bool isExit = false;
    
    // vector<string> => vector<vector<char>> 재구성
    for(int i = 0; i < maps.size(); i++)
    {
        for(int j = 0; j < maps[0].size(); j++)
        {
            if(maps[i][j]== 'S')
                startPos = POS(j,i,0);
            else if(maps[i][j]=='E')
                exitPos = POS(j,i,0);
            
            mapV[i][j] = maps[i][j];
        }
    }
    
    q.push(startPos);
    visited[startPos.y][startPos.x] = true;
    
    while(!q.empty())
    {
        POS curPos = q.front();
        q.pop();
        int curX = curPos.x;
        int curY = curPos.y;
        int curDist = curPos.dist;
        
        if(isLevered && mapV[curY][curX] == 'E')
        {
            isExit = true;
            break;
        }
        
        for(int i= 0; i < 4; i++)
        {
            int nextX = curX + offsetX[i];
            int nextY = curY + offsetY[i];
            
            // 맵 범위 예외 처리
            if(nextX < 0 || nextY < 0 || nextX >= mapV[0].size() || nextY >= mapV.size())
                continue;
            
            // 첫 레버 위치 도착
            if( isLevered == false && mapV[nextY][nextX] == 'L')
            {
                // 레버 체크
                isLevered = true;
                // 방문 기록 초기화
                visited = vector<vector<bool>>(maps.size(), vector<bool>(maps[0].size(), false));
                
                // 큐 클리어
                for(int k = 0; k < q.size(); k++)
                    q.pop();
                
                // 레버를 첫 요소로 추가
                q.push(POS(nextX, nextY, curDist+1));
                dist[nextY][nextX] = curDist+1;
                visited[nextY][nextX] =true;

                // WHILE문 처음부터 시작
                break;
            }
            
            // 벽 예외처리
            if(mapV[nextY][nextX] == 'X')
                continue;
            
            // 방문 위치 예외처리
            if(visited[nextY][nextX] == true)
                continue;
            
            dist[nextY][nextX] = curDist + 1;
            visited[nextY][nextX] = true;
            q.push(POS(nextX, nextY, curDist + 1));
        }
    }
    
    if(isLevered && isExit)
        answer = dist[exitPos.y][exitPos.x];
    else
        answer = -1;
        
    return answer;
}
```
최단 시간을 구하는 문제이기 때문에 BFS로 풀이를 생각했습니다.

시작지점 -> 레버위치 -> 탈출위치

문제에서 레버를 당긴 상태에서 도착 위치에 가야 탈출할 수 있기 때문에

레버 도착 여부와 탈출 위치 여부를 bool 형식으로 체크했습니다.

각 위치에 거리(시간)을 1씩 더하여 저장하여 레버위치, 탈출위치에 도착했으면

dist 이중벡터의 탈출 위치에 저장된 거리를 리턴했습니다.

둘 중 하나라도 도착하지 못한 상태라면 탈출하지 못한 경우익이 때문에 -1을 반환했습니다.<br><Br>


한 위치에서 네 방향을 탐색할 수 있기 때문에 네 방향을 배열로 담아 반복했습니다.

이동하지 못하는 위치의 조건은 다음 위치에 담긴 문자가 'X'(벽) 이거나 맵의 범위를 벗어나는 경우입니다.<BR><bR>

처음 레버에 도착하는 경우를 레버 bool값이 flase인지로 판별하여 

방문여부를 초기화 하여 while문을 레버위치로 다시 시작하게 했습니다.<br><Br>

시작위치나 탈출위치를 레버가 당겨진 상태가 아니라면 방문 가능합니다.

조건에서 X가 담긴 위치만을 예외처리 했기때문에 모든 조건을 충족한다고 생각하였으나,

테스트에서 30퍼센트 케이스가 불통되었습니다.<BR><bR>

## __오답 수정__

레버를 당긴 후 모든 큐의 원소를 초기화하는 과정에서 

for 반복문을 이용한 제거를했었는데 이 부분에서 모든 요소가 제거되지 않아 오류가 발생했습니다.

큐의 모든 원소를 제거할 경우에는 for문을 이용하는 것 보다 while(!q.empty()) 를 사용하여 큐가 빌 때까지 pop을 수행해야 더욱 명확하게 제거할 수 있습니다.

```c++
// 원래 코드
for(int i = 0; i < q.size(); i++)
    q.pop();

// 수정 코드
while(!q.empty())
    q.pop();
}
```

<Br><Br>

## __수정 전체 코드__

```c++
#include <string>
#include <vector>
#include <queue>
#include <iostream>

using namespace std;

struct POS
{
    int x;
    int y;
    int dist;
    POS(int _x, int _y, int _dist){x = _x; y = _y; dist = _dist;}
};

int solution(vector<string> maps) {
    // 미로 탈출 최소 시간
    int answer = 0;
    
    vector<vector<char>> mapV(maps.size(), vector<char>(maps[0].size()));
    vector<vector<int>> dist(maps.size(), vector<int>(maps[0].size()));
    vector<vector<bool>> visited(maps.size(), vector<bool>(maps[0].size(), false));
    
    // 상 우 하 좌
    int offsetX[] = {0, 1, 0, -1};
    int offsetY[] = {-1, 0, 1, 0};
    
    queue<POS> q;
    POS startPos = POS(0,0,0);
    POS exitPos = POS(0,0,0);
    bool isLevered = false;
    bool isExit = false;
    
    // vector<string> => vector<vector<char>> 재구성
    for(int i = 0; i < maps.size(); i++)
    {
        for(int j = 0; j < maps[0].size(); j++)
        {
            if(maps[i][j]== 'S')
                startPos = POS(j,i,0);
            else if(maps[i][j]=='E')
                exitPos = POS(j,i,0);
            
            mapV[i][j] = maps[i][j];
        }
    }
    
    q.push(startPos);
    visited[startPos.y][startPos.x] = true;
    
    while(!q.empty())
    {
        POS curPos = q.front();
        q.pop();
        int curX = curPos.x;
        int curY = curPos.y;
        int curDist = curPos.dist;
        
        if(isLevered && mapV[curY][curX] == 'E')
        {
            isExit = true;
            break;
        }
        
        for(int i= 0; i < 4; i++)
        {
            int nextX = curX + offsetX[i];
            int nextY = curY + offsetY[i];
            
            // 맵 범위 예외 처리
            if(nextX < 0 || nextY < 0 || nextX >= mapV[0].size() || nextY >= mapV.size())
                continue;
            
            // 첫 레버 위치 도착
            if( isLevered == false && mapV[nextY][nextX] == 'L')
            {
                isLevered = true;
                visited = vector<vector<bool>>(maps.size(), vector<bool>(maps[0].size(), false));
                
                //for(int k = 0; k < q.size(); k++)
                while(!q.empty())
                    q.pop();
                
                q.push(POS(nextX, nextY, curDist+1));
                dist[nextY][nextX] = curDist+1;
                visited[nextY][nextX] =true;
                break;
            }
            
            // 벽 예외처리
            if(mapV[nextY][nextX] == 'X')
                continue;
            
            // 방문 위치 예외처리
            if(visited[nextY][nextX] == true)
                continue;
            
            dist[nextY][nextX] = curDist + 1;
            visited[nextY][nextX] = true;
            q.push(POS(nextX, nextY, curDist + 1));
        }
    }
    
    if(isLevered && isExit)
    {
        answer = dist[exitPos.y][exitPos.x];
    }
    else
    {
        answer = -1;
    }
        
    return answer;
}
```
