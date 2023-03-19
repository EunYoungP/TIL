[2023.03.03](#나의-풀이참고)


# __[프로그래머스 LV3] N으로 표현__

DFS/DP

---- 

<BR>

## __문제__

<img src = "https://user-images.githubusercontent.com/80774412/226182538-88fb4167-ddf5-40f0-8b38-e0318a261c80.PNG"></img>
<BR><BR>

<img src = "https://user-images.githubusercontent.com/80774412/226182540-6aaaec33-a856-493d-a2ab-4484edb181ff.PNG"></img>
<BR><BR>



## __나의 풀이__(오답)
```c++
#include <string>
#include <vector>
#include <queue>
#include <algorithm>
#include <iostream>

using namespace std;

struct POS
{
    int x, y, dist;
    
    POS(int _x, int _y, int _dist){x = _x; y = _y;dist = _dist;}
};

// 오른쪽, 아래
int offsetX[2] = {1, 0};
int offsetY[2] = {0, 1};

void DFS(int mapX, int mapY, vector<vector<bool>> puddlesV, vector<vector<bool>> visited, POS pos, int& minCount, int& minDist)
{
    // 목적지 도착
    if(pos.x == mapX-1 && pos.y == mapY-1)
    {
        // 새로운 최단 경로
        if(pos.dist < minDist)
        {
            minCount = 0;
            minCount++;
            minDist = pos.dist;
        }
        // 기존 최단 경로
        else if(pos.dist == minDist)
        {
            minCount++;
        }
        return;
    }

    int curX = pos.x;
    int curY = pos.y;
    visited[curY][curX] = true;
    for(int i = 0; i < 2; i ++)
    {
        int newX = curX + offsetX[i];
        int newY = curY + offsetY[i];

        // 격자 범위 체크
        if(newX < 0 || newY < 0 || newX >= mapX || newY >= mapY)
            continue;

        // 방문 여부 체크
        if(visited[newY][newX] == true)
            continue;

        // 침수 지역 체크
        if(puddlesV[newY][newX] == true)
            continue;

        DFS(mapX, mapY, puddlesV, visited, POS(newX,newY,pos.dist+1), minCount, minDist);
    }
}

int solution(int m, int n, vector<vector<int>> puddles) {
    int answer = 0;
    
    vector<vector<bool>> visited(n, vector<bool>(m));
    vector<vector<bool>> puddlesV(n, vector<bool>(m));
    int minDist = 1000;
    int minCount = 0;
    
    // puddles 벡터에 침수 : 1, 침수X : 0
    for(int i = 0 ; i < puddles.size(); i++)
    {
        int x = puddles[i][0];
        int y = puddles[i][1];
        puddlesV[y-1][x-1] = true;
    }
    
    visited[0][0] = true;
    DFS(m, n, puddlesV, visited, POS(0,0,0), minCount, minDist);
    
    return minCount;
}
```

### __결과__

<img src = "https://user-images.githubusercontent.com/80774412/226182542-78315bb3-eedd-4f7a-a0f2-0bfabae3606e.PNG"></img>
<BR><BR>

처음에는 최단 경로로 문제를 이해하고 BFS로 풀이하였으나,

최단 경로가 아닌 최단 경로의 갯수를 구하는 문제였기 때문에 

모든 경로를 검색하여 최단 경로인지 알아야 하고, 다양한 경로 각각의 탐색 길이를 가져야하기 때문에 DFS 풀이로 변경했습니다.<BR><bR>

간단한 테스트와 정확성 테스트는 한 개가 시간 초과로 실패했고, 

효율성 테스트는 모두 시간 초과로 실패했습니다.

좀 더 시간복잡도가 낮은 방법을 찾아야 했습니다.
