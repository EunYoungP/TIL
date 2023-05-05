2023.05.04

# __[프로그래머스 LV2] 혼자하는 틱택토__


## __문제__

[혼자하는 틱택토](https://school.programmers.co.kr/learn/courses/30/lessons/160585)<br><Br>

## __나의 풀이__(오답)
```c++
#include <string>
#include <vector>

using namespace std;

int solution(vector<string> board) {
    // 게임 진행 순서는 0 다음 X
    // 빙고가 나오면 게임 종료
    // 칸이 다 채워지면 게임 종료
    
    // 게임판이 가능한 경우 1, 아닐경우 0
    int answer = 1;
    
    // 가능하지 않은 경우
    
    // 1. X와 O의 수의 차이가 2 이상일 경우
    int Xnum = 0;
    int Onum = 0;
    for(int i =0; i < board.size(); i++)
    {
        for(int j = 0; j < board[0].size(); j++)
        {
            if(board[i][j] == 'O')
                Onum++;
            else if(board[i][j] =='X')
                Xnum++;
        }
    }
    
    if(Onum < Xnum || (Onum - Xnum) >= 2)
    {
        return 0;
    }
    
    // 2. 3 빙고가 탄생했을 경우
    int XBingo = 0;
    int OBingo = 0;
    for(int i= 0; i < 3; i++)
    {
        // 가로 세 줄
        string horizontalLine = board[i];
        if(horizontalLine[0] == horizontalLine[1] && horizontalLine[1] == horizontalLine[2])
        {
            if(horizontalLine[0] == 'X')
                XBingo++;
            else if(horizontalLine[0] == 'O')
                OBingo++;
        }
        
        // 세로 세 줄
        if(board[0][i] == board[1][i] && board[1][i] == board[2][i])
        {
            if(board[0][i] == 'X')
                XBingo++;
            else if(board[0][i] == 'O')
                OBingo++;
        }
    }
                                        
    // 대각선 두 줄
    if(board[0][0] == board[1][1] && board[1][1] == board[2][2])
    {
        if(board[0][0] == 'X')
            XBingo++;
        else if(board[0][0] == 'O')
                OBingo++;
    }
                    
    if(board[2][0] == board[1][1] && board[1][1] == board[2][0])
    {
        if(board[0][0] == 'X')
            XBingo++;
        else if(board[0][0] == 'O')
            OBingo++;
    }
    
    // 2-1. X빙고일 경우 O는 X와 같은 개수여야 합니다.
    if(XBingo > 0)
    {
        if(Xnum != Onum)
            return 0;
    }
                    
    // 2-2. O빙고일 경우 X는 O보다 1적어야 합니다.
   if(OBingo > 0)
    {
        if((Onum - Xnum) != 1)
            return 0;
    }
           
    // 2-3. X, O빙고가 동시에 존재할 수 없습니다.
    if(XBingo >0 && OBingo > 0)
        return 0;
    
    return answer;
}
```

보드의 칸이 9개 밖에 안되기 때문에 모든 경우의 수에서

보드가 불가능한 상태일 경우를 예외처리 하여 0을 리턴했습니다.

오류가 발생헀는데 인덱스의 오류때문에 발생했습니다.

아래처럼 수정해주었습니다.<br><Br>

__기존 코드__
```c++
    if(board[2][0] == board[1][1] && board[1][1] == board[2][0])
    {
        if(board[0][0] == 'X')
            XBingo++;
        else if(board[0][0] == 'O')
            OBingo++;
    }
    
```
<Br><Br>

__수정 후__
```c++
    if(board[2][0] == board[1][1] && board[1][1] == board[0][2])
    {
        if(board[2][0] == 'X')
            XBingo++;
        else if(board[2][0] == 'O')
            OBingo++;
    }
```
<Br><Br>

## __수정 코드 전체__

```c++
#include <string>
#include <vector>

using namespace std;

int solution(vector<string> board) {
    // 게임 진행 순서는 0 다음 X
    // 빙고가 나오면 게임 종료
    // 칸이 다 채워지면 게임 종료
    
    // 게임판이 가능한 경우 1, 아닐경우 0
    int answer = 1;
    
    // 가능하지 않은 경우
    
    // 1. X와 O의 수의 차이가 2 이상일 경우
    int Xnum = 0;
    int Onum = 0;
    for(int i =0; i < board.size(); i++)
    {
        for(int j = 0; j < board[0].size(); j++)
        {
            if(board[i][j] == 'O')
                Onum++;
            else if(board[i][j] =='X')
                Xnum++;
        }
    }
    
    if(Onum < Xnum || (Onum - Xnum) >= 2)
    {
        return 0;
    }
    
    // 2. 빙고가 탄생했을 경우
    int XBingo = 0;
    int OBingo = 0;
    int horizontal = 0;
    int vertical = 0;
    for(int i= 0; i < 3; i++)
    {
        // 가로 세 줄
        string horizontalLine = board[i];
        if(horizontalLine[0] == horizontalLine[1] && horizontalLine[1] == horizontalLine[2])
        {
            if(horizontalLine[0] == 'X')
            {
                XBingo++;
                horizontal++;
            }
                
            else if(horizontalLine[0] == 'O')
            {
                OBingo++;
                horizontal++;
            }
        }
        
        // 세로 세 줄
        if(board[0][i] == board[1][i] && board[1][i] == board[2][i])
        {
            if(board[0][i] == 'X')
            {
                vertical++;
                XBingo++;
            }
            else if(board[0][i] == 'O')
            {
                vertical++;                
                OBingo++;
            }
        }
    }
    
    if(horizontal >= 2 || vertical >= 2)
        return 0;
                                        
    // 대각선 두 줄
    if(board[0][0] == board[1][1] && board[1][1] == board[2][2])
    {
        if(board[0][0] == 'X')
            XBingo++;
        else if(board[0][0] == 'O')
                OBingo++;
    }
                    
    if(board[2][0] == board[1][1] && board[1][1] == board[0][2])
    {
        if(board[2][0] == 'X')
            XBingo++;
        else if(board[2][0] == 'O')
            OBingo++;
    }
    
    // 2-1. X, O빙고가 동시에 존재할 수 없습니다.
    if(XBingo > 0 && OBingo > 0)
        return 0;
    
    // 2-2. X빙고일 경우 O는 X와 같은 개수여야 합니다.
    if(XBingo > 0)
    {
        if(Xnum != Onum)
            return 0;
    }
                    
    // 2-3. O빙고일 경우 X는 O보다 1적어야 합니다.
   if(OBingo > 0)
    {
        if((Onum - Xnum) != 1)
            return 0;
    }
    return answer;
}
```
