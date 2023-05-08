2023.05.08

# __[프로그래머스 LV2] 무인도 여행__

BFS탐색

----

## __문제__

[무인도 여행](https://school.programmers.co.kr/learn/courses/30/lessons/154540)<br><Br>

## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

struct POS
{
    int x; 
    int y;
    POS(int _x, int _y){x = _x; y = _y;}
};

vector<int> solution(vector<string> maps) {
    
    // 상 우 하 좌
    int offsetX[] = {0, 1, 0, -1};
    int offsetY[] = {-1, 0, 1, 0};
    
    vector<int> answer;
    // 식량 많은 오름차순
    // 없으면 -1
    
    queue<POS> q;
    vector<vector<bool>> visited(maps.size(), vector<bool>(maps[0].size()));
    
    for(int i = 0; i < maps.size(); i++)
    {
        for(int j= 0; j < maps[i].size(); j++)
        {
            if(maps[i][j] != 'X' && visited[i][j] == false)
            {
                int food = 0;
                q.push(POS(j,i));
                visited[i][j] = true;
                food += maps[i][j] - '0';
                
                while(!q.empty())
                {
                    POS curPos = q.front();
                    int curX = curPos.x;
                    int curY = curPos.y;

                    q.pop();

                    for(int i = 0; i < 4; i++)
                    {
                        int nextX = curX + offsetX[i];
                        int nextY = curY + offsetY[i];

                        if(nextX < 0 || nextX >= maps[0].size() || nextY < 0 || nextY >= maps.size())
                            continue;

                        if(visited[nextY][nextX] == true)
                            continue;

                        if(maps[nextY][nextX] == 'X')
                            continue;

                        q.push(POS(nextX, nextY));
                        visited[nextY][nextX] = true;
                        food += maps[nextY][nextX] - '0';
                    }
                }
                answer.push_back(food);
            }
        }
    }
    
    sort(answer.begin(), answer.end());
    if(answer.size() <= 0)
        answer.push_back(-1);
    
    return answer;
}
```

하나의 지도에 여러개의 무인도가 존재합니다.

기본적인 BFS탐색 문제라면 무인도가 하나 존재하여 무인도인 칸의 식량 수를 구하는 식이였겠지만,

여기서 무인도는 하나가 아니기 때문에

각 무인도마다 BFS탐색을 하여 연결된 칸들을 찾아줘야합니다.<bR><bR>

떄문에 우선 무인도인 칸을 하나 찾습니다.

이 칸을 시작으로 연결된 무인도 땅을 검색하여 방문 여부를 체크해줍니다.

그리고 다음 무인도섬의 시작점을 찾을 떄는 방문했던 위치는 패스하는 식으로 풀이했습니다.<BR><bR>

단지 주의해야 할 점은 답의 형식은 int 이고 지도의 식량 수는 char 이기 때문에 형변환을 해줘야합니다.

위 풀이에서는 char형식에서 문자 '0'을 뺴주어 자동으로 int 형변환이 이루어집니다.