2023.05.31

# __[프로그래머스 LV2] 이모티콘 할인 행사__

완전탐색, DFS

----

## __문제__

[이모티콘 할인 행사](https://school.programmers.co.kr/learn/courses/30/lessons/150368#qna)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <map>
#include <iostream>

using namespace std;

int Percent[4] = {10, 20, 30, 40};

void dfs( int cur, int max, vector<vector<int>>& answers, vector<int> answer)
{
    if(cur >= max)
    {
        answers.push_back(answer);
        return;
    }
    
    for(int i = 0; i < 4; i++)
    {
        answer.push_back(Percent[i]);
        //cout << "추가될 퍼센트는 : " << Percent[i] <<endl;
        dfs(cur+1, max, answers, answer);
    }
}

// 사용자 구매 기준[비율, 가격]
vector<int> solution(vector<vector<int>> users, vector<int> emoticons) {
    
    // 목표 1. 가입자 최대한 늘리기
    // 목표 2. 판매액 최대한 늘리기
    // 최대의 가입수, 매출액
    vector<int> answer{0,0};
    
    // 답 후보 리스트
    vector<vector<int>> answers;
    vector<int> temp;
    // 가입수를 늘리려면, 최대한 많은 구매기준을 만족해야합니다.
    // 4개의 할인비율 종류
    // 최대 8개의 이모티콘 종류
    // n명의 구매기준
    
    // 모든 경우의 수를 검색하기위해 dfs를 실행하고
    dfs(0, emoticons.size(), answers, temp);

    // 모든 퍼센트 경우의 수를 이모티콘 리스트에 대입합니다.
    // map에 정렬합니다.<퍼센트수, 가격>
    // 모든 경우의 수
    for(int i = 0; i < answers.size(); i++)
    {
        map<int, int> emap;
        for(int j= 0; j < answers[i].size(); j++)
        {
            // 하나의 비율 경우의 수를 대입한 
            // 이모티콘의 <퍼센트, 가격>
            emap[answers[i][j]] += (emoticons[j] / 100) * (100-answers[i][j]);
            //cout << "map값은 " << emap[answers[i][j]] << " 퍼센트는 : " << answers[i][j] << endl;
        }
        
        int joinNum = 0;
        int sales = 0;
        int temp = 0;
        
        // 사용자 구매 기준을 순회하며
        // map의 요소들을 검색하여 가장 많이 가입하는 경우를 구합니다.
        for(int u = 0; u < users.size(); u++)
        {
            int minPercent = users[u][0];
            int minPrice = users[u][1];
            
            for(int m = 0 ; m < 4; m++)
            {
                if(minPercent <= Percent[m])
                {
                    if(emap.find(Percent[m])!=emap.end())
                    {
                        temp += emap[Percent[m]];
                        //cout << "찾은 가격은 : " << emap[Percent[m]] << endl;
                    }
                }
            }
            
            if(minPrice <= temp)
            {
                //cout << "최소값 이상 : " << joinNum << endl;
                joinNum++;
            }
            else
            {
                sales += temp;
                //cout << "최소값 미만입니다. " << "최소값 : " << minPrice << " 지불금액 : " << sales << endl;
            }
        }
        
        // 가장 많이 가입하는 경우들 중
        // 가장 판매액이 높은 경우의 수를 추려냅니다.
        if(answer[0] < joinNum)
        {
            answer[0] = joinNum;
            answer[1] = sales;
        }
        else if(answer[0] == joinNum)
        {
            if(answer[1] < sales)
            {
                answer[1] = sales;
            }
        }
    }
    return answer;
}
```

