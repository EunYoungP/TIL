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

인덱스의 오류가 발생하는지, 출력테스트에서 temp의 누적합이 나올 수 없는 -값이 나오는 결과가 발생했습니다.

맨 처음 answers의 값을 출력하였을 떄, answers의 내부 벡터의 사이즈가 이모티콘의 사이즈와 같아야하는데 한 두개씩 차이나는 것을 보아,

dfs에서 answers에 모든 경우의 수를 할당할때 문제가 생긴것으로 파악했습니다.

<Br><Br>

## __오류 수정__

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
        dfs(cur+1, max, answers, answer);
        // 오류 수정, dfs에서 모든 경우를 검색하기 위해 다음 분기검색 전 추가한 값을 삭제해줘야 합니다.
        answer.erase(answer.end()-1);
    }
}

// 사용자 구매 기준[비율, 가격]
vector<int> solution(vector<vector<int>> users, vector<int> emoticons) {
    
    // 목표 1. 가입자 최대한 늘리기
    // 목표 2. 판매액 최대한 늘리기
    // 최대의 가입수, 매출액
    vector<int> answer{0,0};
    
    // 답이 될수있는 목록
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
            emap[answers[i][j]] += (emoticons[j] / 100) * (100 - answers[i][j]);
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
            
            temp = 0;
            for(int m = 0 ; m < 4; m++)
            {
                if(minPercent <= Percent[m])
                {
                    if(emap.find(Percent[m]) != emap.end())
                    {
                        temp += emap[Percent[m]];
                    }
                }
            }
            
            if(minPrice <= temp)
            {
                joinNum++;
            }
            else
            {
                sales += temp;
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

어떠한 문제를 풀기 전에 시간 초과 현상을 고려하여 어떤 알고리즘으로 해결해야 할지 고민하게됩니다.

이런 경우 1억번의 계산이 필요하지 않은 이상은 대부분 시간 초과가 나지 않습니다.<br><BR>

위 문제에서 모든 퍼센트의 최대 경우의 수의 개수는

최대 이모티콘의 개수 8 * 최대 사용자의 수 100 에 퍼센트의 종류가 4가지 이므로 4*3*2 인 24를 곱해주는 값입니다.

즉, 7 * 100 * 24 = 약 16000

<br><Br>

최대 계산을 알아보면,

7 * 100 * 16000 = 약 11,000,000

천백만번의 계산수가 나오므로 1억번보다 훨씬 적으므로 완전탐색으로 계산해도 무리가 없다는 결론이 나옵니다.<br><Br>

따라서 문제에 따라 코드를 만들어주면 풀이되는 문제였습니다.<br><BR>

1. dfs를 통해 모든 이모티콘이 가질 수 있는 퍼센트에대한 경우의 수를 구해줍니다.

2. 모든 경우의 수를 순회하며 map에 <퍼센트, 퍼센트만큼 할인된 값>으로 정리해줍니다.

3. 퍼센트 경우의 수의 각 분기에서 유저를 순회합니다.

    3-1. 유저의 최소 퍼센트, 최소 가격 기준을 비교해줍니다.

        최소 퍼센트보다 할인률이 높고, 그에 따른 가격의 합이 높다면 가입자수를 표현하는 joinNum을 증가시켜줍니다.

        그 예외의 경우, sales에 값을 누적시켜줍니다.

4. joinNum, sales를 answer의 각 요소와 비교하여 조건을 만족한다면 할당합니다.

    마지막에 남는 answer의 값들이 최대 가입자수, 최대 매출로 도출됩니다.

