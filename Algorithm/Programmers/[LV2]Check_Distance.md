2023.02.08

# __[프로그래머스 LV2] 거리두기 확인하기__


2021 kakao 채용연계형 인턴십

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
            isDiagonal = true;
            return CheckDistance(x, y, room, isDiagonal)
        }
    }
    
    // 4 direction
    for(int i = 0; i < 4; i++)
    {
        int nx = x + offsetX[i];
        int ny = y + offsetY[i];
        
        char checkAround = room[ny][nx];
        
        if(checkAround == 'P')
            return true;
        
        int nnx = x + (offsetX[i]*2);
        int nny = y + (offsetY[i]*2);
        
        char checkAroundDouble = room[nny][nnx];
        
        if(checkAroundDouble == 'P')
            return true;
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