2023.02.08

# __[프로그래머스 LV2] 거리두기 확인하기__

2021 kakao 채용연계형 인턴십

bfs / dfss

---- 

## __문제__

<img src="https://user-images.githubusercontent.com/80774412/217560901-e1be3c9e-66a0-431d-8fd6-decdcf8b7b77.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/217560910-43647278-33b5-4776-b026-144d8239b128.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/217560927-9885f85e-4cc2-462b-9d71-b246b20a278a.PNG"></img>

<br>

## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <map>

using namespace std;

struct Place
{
    int x, y;
    Place(int _x int _y){x = _x; y = _y;}
};

enum DisType
{
    diagonal,
    direction,
    doubledirection,
}

// 우하, 좌하, 좌상, 우상
int diagonalX[] = {1,-1,-1,1};
int diagonalY[] = {1,1,-1,-1};

// 상 우 하 좌
int offsetX[] = {0, 1, 0, -1};
int offsetY[] = {-1, 0, 1, 0};

bool CheckDistance(Place curPlace, Place comparePlace, vector<string> room, DisType disType)
{
    if(disType == DisType.diagonal)
    {
        
    }
        
    else if(disType == DisType.direction)
    {
        
    }
        
    else if(disType == DisType.doubledirection)
    {
        
    }
}

bool CheckPInManhattanDis(int x, int y, vector<string> room, bool& isDiagonal)
{
    // diagonal
    for(int i = 0; i < 4; i++)
    {
        int nx = x + diagonalX[i];
        int ny = y + diagonalY[i];
        
        char checkAround = room[ny][nx];
        if(checkAround == 'P')
        {
            return CheckDistance(x, y, room, isDiagonal, DisType.diagonal);
        }
    }
    
    // 4 direction
    for(int i = 0; i < 4; i++)
    {
        int nx = x + offsetX[i];
        int ny = y + offsetY[i];
        
        char checkAround = room[ny][nx];
        
        if(checkAround == 'P')
        {
            return true;
        }
        
        int nnx = x + (offsetX[i]*2);
        int nny = y + (offsetY[i]*2);
        
        char checkAroundDouble = room[nny][nnx];
        
        if(checkAroundDouble == 'P')
        {
            return true;
        }
    }
    return false;
}

vector<int> solution(vector<vector<string>> places) {
    // 각 대기실의 거리두기 실천여부
    vector<int> answer(5);
    bool isDiagonal;
    map<pair<int, int>, vector<int>> checked;
    
    // 방 5개를 검사합니다.
    for(int i = 0; i <places.size(); i++)
    {
        for(int j = 0; j < places[i].size(); j++)
        {
            if(places[i][j] == "P")
                // 맨해튼 범위에 사람이 있는지 검사합니다.
                if(CheckPInManhattanDis(j, i, places[i], isDiagonal))
                {
                    answer[i] = 0;
                }
        }
        answer[i] = 1;
    }
    return answer;
}
```
bfs / dfs 를 사용하지 않고 조건문으로만 풀려고 하다보니 코드가 너무 길어져 앞서 말한 방법으로 구현했습니다.
<br><br>

## __dfs 재풀이__(오답)
```c++
#include <string>
#include <vector>
#include <iostream>

using namespace std;

struct Place
{
    int x, y;
    Place(int _x, int _y){x = _x; y = _y;}
};

// 우하, 좌하, 좌상, 우상
int diagonalX[] = {1,-1,-1,1};
int diagonalY[] = {1,1,-1,-1};

// 상 우 하 좌
int offsetX[] = {0, 1, 0, -1};
int offsetY[] = {-1, 0, 1, 0};

bool dfs(vector<string> room, vector<vector<bool>>& visited, int x, int y, int depth)
{
    
    if(depth >= 2)
        return true;
    cout << " " << y << " " << x << endl;
    visited[y][x] = true;
    for(int i = 0; i < 4; i++)
    {
        int nx = x + offsetX[i];
        int ny = y + offsetY[i];
        
        if(nx < 0 || ny < 0 || nx >= 5 || ny >= 5)
            continue;
        if(visited[ny][nx] == true)
            continue;
        
        if(room[ny][nx] == 'P')
            return false;
        
        
        dfs(room, visited, nx, ny, depth+1);
    }
    return true;
}
    
vector<int> solution(vector<vector<string>> places) {
    // 각 대기실의 거리두기 실천여부
    vector<int> answer(5, 1);
    vector<vector<bool>> visited(5,vector<bool>(5));
    
    for(int i = 0; i < 5; i++)
    {
        vector<string> curRoom = places[i];
        for(int j = 0; j < 5; j++)
        {
            for(int n = 0; n < 5; n++)
            {
                if(curRoom[j][n] == 'P')
                {
                    visited[j][n] = true;
                    if(!dfs(curRoom, visited, n, j, 0))
                    {
                       answer[i] = 0; 
                    }
                       
                }
            }
        }
    }
    return answer;
}
```
구현하긴 했는데 dfs 함수에서 'P'를 만났을때 false를 리턴해주는 부분의 조건이 한번도 실행되지 않았습니다.



-------
# __[프로그래머스 LV2] 주식 가격__

## __나의 풀이__(정답)

```C++
#include <string>
#include <vector>

using namespace std;

vector<int> solution(vector<int> prices) {
    vector<int> answer(prices.size(), 0);
    
    for(int i = 0; i < prices.size(); i++)
    {
        int count = 0;
        int curPrice = prices[i];
        for(int j = i; j < prices.size()-1; j++)
        {
            if(curPrice <= prices[j])
                count++;
            else
                break;
        }
        answer[i] = count;
    }
    return answer;
}
```