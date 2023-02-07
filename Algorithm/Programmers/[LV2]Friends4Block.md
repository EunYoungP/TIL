2023.02.07

# __[프로그래머스 LV2] 프렌즈4블록__


[kakao신입 공채 문제 해설]("https://tech.kakao.com/2017/09/27/kakao-blind-recruitment-round-1/#6-%ED%94%84%EB%A0%8C%EC%A6%884%EB%B8%94%EB%A1%9D%EB%82%9C%EC%9D%B4%EB%8F%84-%EC%83%81")

---- 

## __문제__

<img src="https://user-images.githubusercontent.com/80774412/217279160-b9a236a4-ff1a-43dd-ad1e-a98d1f55b9a9.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/217279181-06c5284c-f892-444d-bf64-f80da393df9f.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/217279203-872e74e8-fdd3-4a2c-a91a-e29fcc4e24f5.PNG"></img>

<img src="https://user-images.githubusercontent.com/80774412/217279215-69ef2e13-34c3-4adf-b165-cd448c417719.PNG"></img>

<br><br>

## __나의 풀이__(오답)

```c++
#include <string>
#include <vector>
#include <iostream>

using namespace std;

// 우 하 우하
int diagonalX[] = {1,0,1};
int diagonalY[] = {0,1,1};

int M;
int N;

struct Block
{
    int x;
    int y;
    Block(int _x, int _y){x = _x; y = _y;}
};

bool CheckBlock(int m, int n, vector<string> board)
{
    char curType = board[m][n];
    
    for(int i = 0; i < 3; i++)
    {
        int checkX = n + diagonalX[i];
        int checkY = m + diagonalY[i];
        
        if(checkX < 0 || checkY < 0 || checkX >= N || checkY >= M)
        {
            //cout << "범위 벗어남" << endl;
            return false;
        }
            
        
        if(board[checkY][checkX] != curType)
        {
            //cout << "같은 형식 아님" << endl;
            return false;
        }
    }
    //cout << "체크 완료" << endl;
    return true;
}

int DeleteBlock(vector<Block> deleteBlocks, vector<string>& board)
{
    int deleteCnt = 0;
    for(int i = 0; i < deleteBlocks.size(); i++)
    {
        int x = deleteBlocks[i].x;
        int y = deleteBlocks[i].y;
        
        if(board[y][x] != '.')
        {
            board[y][x] = '.';
            deleteCnt++;
        }
        
        for(int j = 0; j < 3; j++)
        {
            int nx = x + diagonalX[i];
            int ny = y + diagonalY[i];
            if(board[ny][nx] != '.')
            {
                board[ny][nx] = '.';
                deleteCnt++;
            }
        }
    }
    return deleteCnt;
}

void SortBlock(vector<string>& board)
{
    for(int i = M-1; i >= 0; i--)
    {
        for(int j = 0; j < N; j++)
        {
            if(board[i][j] == '.')continue;
            
            int ny = i+1;
            while(ny < M && board[ny][j] == '.')ny++;
            ny--;
            if(ny != i)
            {
                board[ny][j] = board[i][j];
                board[i][j] = '.';
            }
        }
    }
}

int solution(int m, int n, vector<string> board) {
    int answer = 0;
    
    M = board.size();
    N = board[0].size();
    
    bool doneMatch = false;
    
    while(!doneMatch)
    {
        doneMatch = true;
        vector<Block> matchBlock;
        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                // 주변 3개의 블록과 같은 유형인지 검사합니다.
                if(CheckBlock(i,j,board))
                {
                    // 콤보로 매치되는 상태에서 빈 블럭들의 검색을 제외합니다.
                    if(board[i][j] == '.')continue;
                    matchBlock.push_back(Block(i, j));
                    doneMatch = false;
                }
            }
        }
        
        if(!doneMatch)
        {
            // 삭제할 블록들의 개수를 구해서 답에 더해줍니다.
            answer = answer + DeleteBlock(matchBlock, board);
            // 삭제된 칸과 위에 남아있는 블록칸을 바꿔서 정렬해줍니다.
            SortBlock(board);
        }
    }
    return answer;
}
```

<br><Br>