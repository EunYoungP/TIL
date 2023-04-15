2023.04.15

# __[프로그래머스 LV3] 숫자 타자 대회__



## __문제__

[숫자 타자 대회](https://school.programmers.co.kr/learn/courses/30/lessons/136797#qna)<br><Br>

## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <queue>
#include<algorithm>

using namespace std;

struct POS
{
    int x, y;
    POS(int _x, int _y){x = _x; y = _y;}
};

int solution(string numbers) {
    int answer = 0;
    
    // 시작 : 상, 시계방향
    int XOffset[] = {0, 1, 1, 1, 0, -1, -1, -1};
    int YOffset[] = {-1, -1, 0, 1, 1, 1, 0, -1};
    
    vector<vector<int>> numbersVector = {{1,2,3},{4,5,6},{7,8,9},{10,0,11}};
    vector<vector<bool>> visited(4, vector<bool>(3));
    POS curLeftThumb = POS(0,1);
    POS curRightThumb = POS(2,1);
    
    queue<POS> q;
    
    for(int i = 0; i < numbers.size(); i++)    
    {
        int LeftDist = 0;
        int RightDist = 0;
        
        // 왼손 엄지 최단거리 검사
        q.push(curLeftThumb);
        while(!q.empty())
        {
            POS curPos = q.front();
            q.pop();
            
            int curX = curPos.x;
            int curY = curPos.y;
            visited[curY][curX] = true;
            
            if(LeftDist == 0 && numbers[i] == numbersVector[curY][curX])
            {
                LeftDist = 1;
                continue;
            }
            
            if(numbers[i] == numbersVector[curY][curX])
                continue;
            
            for(int j = 0; j < 8; j++)
            {
                int newX = curX + XOffset[j];
                int newY = curY + YOffset[j];
                
                if(newX < 0 || newX >= numbersVector[0].size() || newY < 0 || newY >= numbersVector.size())
                    continue;
                
                if(visited[newY][newX] == true)
                {
                     continue;
                }
                
                // 대각 이동일 경우
                if((j % 2) == 1)
                {
                    LeftDist += 3;
                }
                else
                {
                    LeftDist += 2;
                }
                q.push(POS(newX, newY));
            }
        }
        
        // 멤버 초기화
        q = queue<POS>();
        q.push(curRightThumb);
        visited = vector<vector<bool>>(4, vector<bool>(3));
        
        // 오른손 엄지 최단거리 검사
        while(!q.empty())
        {
            POS curPos = q.front();
            q.pop();
            
            int curX = curPos.x;
            int curY = curPos.y;
            visited[curY][curX] = true;
                        
            if(RightDist == 0 && numbers[i] == numbersVector[curY][curX])
            {
                RightDist = 1;
                continue;
            }
            
            if(numbers[i] == numbersVector[curY][curX])
                continue;
            
            for(int j = 0; j < 8; j++)
            {
                int newX = curX + XOffset[j];
                int newY = curY + YOffset[j];
                
                if(newX < 0 || newX >= numbersVector[0].size() || newY < 0 || newY >= numbersVector.size())
                    continue;
                
                if(visited[newY][newX] == true)
                {
                     continue;
                }
                
                // 대각 이동일 경우
                if((j % 2) == 1)
                {
                    RightDist += 3;
                }
                else
                {
                    RightDist += 2;
                }
                q.push(POS(newX, newY));
            }
        }
        // 왼손 오른손 최단거리 비교
        answer += min(LeftDist, RightDist);
    }
    return answer;
}
```

왼손가락과 오른손가락을 시작점으로 한 BFS풀이로 주어진 nubmers 각 목표에 대한 가중치 비교로 매번 비교하여 풀이하였습니다.

이렇게 풀이하면 왼손 오른손 각각의 BFS 진행으로 인해 두배가 되어 시간초과가 예상되었지만,

시간초과는 나지 않았지만 값 정확도에 대한 오류가 발생했습니다.