2023.05.02

# __[프로그래머스 LV2] 리코쳇 로봇__


## __문제__

[리코쳇 로봇](https://school.programmers.co.kr/learn/courses/30/lessons/169199)<br><Br>

## __나의 풀이__(정답)
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
    
    POS(int _x , int _y, int _dist){x = _x; y = _y; dist = _dist;}
};

int solution(vector<string> board) {
    int answer = 0;
    
    int offsetX[] = {0, 1, 0, -1};
    int offsetY[] = {-1, 0, 1, 0};
    
    vector<vector<char>> boardV(board.size(), vector<char>(board[0].size()));
    vector<vector<bool>> visited(board.size(), vector<bool>(board[0].size(), false));
    vector<vector<int>> dist(board.size(), vector<int>(board[0].size(), 0));
    
    queue<POS> q; 
    POS startPos = POS(0,0,0);
    POS endPos = POS(0,0,0);
    
    // vector<string> -> vector<vector<int>>
    for(int i = 0; i < board.size(); i++)
    {
        for(int j = 0; j < board[i].size(); j++)
        {
            if(board[i][j] == 'R')
                startPos = POS(j,i,0);
            if(board[i][j] == 'G')
                endPos = POS(j,i,0);
            
            boardV[i][j] = board[i][j];        
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
        
        if(boardV[curY][curX] == 'G')
            return dist[curY][curX];
        
        for(int i= 0; i < 4; i++)
        {
            int nextX = curX; 
            int nextY = curY; 
            
            // 벽이 나오거나
            // 장애물이 나오거나
            // 목적지에 도착할때까지 해당 방향으로 전진
            bool isDone = false;
            while(!isDone)
            {
                nextX += offsetX[i];
                nextY += offsetY[i];
                
                // 벽에 막히거나, 장애물에 막혔을 경우
                if(nextY < 0 || nextX < 0 || nextY >= boardV.size() || nextX >= boardV[0].size()
                  || boardV[nextY][nextX] == 'D')
                {
                    isDone = true;
                    nextX -= offsetX[i];
                    nextY -= offsetY[i];
                }
            }
            
            // 방문여부 검사
            if(visited[nextY][nextX] == true)
                continue;
            else
            {
                visited[nextY][nextX] = true;
                dist[nextY][nextX] = curDist+1;
                q.push(POS(nextX, nextY, curDist+1));
            }
        }
    }
    if(dist[endPos.y][endPos.x] == 0)
        answer = -1;
    else
        answer = dist[endPos.y][endPos.x];
        
    return answer;
}
```

최소 움직임 횟수를 구하는 문제이기떄문에 최단거리를 구하는데 효율적인 BFS탐색으로 문제를 풀이했습니다.

기본적인 BFS는 네 방향을 한칸씩만 검사하며 진행되지만, 

위 문제에서는 벽에 부딪히거나, 'D'칸에 부딪힐때까지라는 조건을 추가로 가지고 있습니다.

때문에 FOR문으로 네 방향을 검사해주고

각각의 반복문에서 벽이나 D칸에 닿을때까지 While문으로 반복해 도착지점을 구했습니다.

그리고 해당 도착지점의 방문여부를 검사해 탐색을 진행했습니다.

또한 각 도착 POS에 dist 변수를 추가하여 거리값을 전달하였습니다.