2023.04.26

# __[프로그래머스 LV2] 과제 진행하기__


## __문제__

[과제 진행하기](https://school.programmers.co.kr/learn/courses/30/lessons/176962)<br><Br>

## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <stack>
#include <algorithm>

using namespace std;

bool compare(vector<string> a, vector<string> b)
{
    return stoi(a[1]) < stoi(b[1]);
}

bool CompareTime(string a, string b)
{
    int aTime = stoi(a.substr(0,2))*60 + stoi(a.substr(3,2));
    int bTime = stoi(b.substr(0,2))*60 + stoi(b.substr(3,2));
    
    return a > b;
}

string GetTime(string a, string b)
{
    int time = stoi(a.substr(0,2));
    int min = stoi(a.substr(3,2));
    
    time += (min + stoi(b))/60;
    min += (min + stoi(b)) % 60;
           
    string temp = to_string(time) + ":" + to_string(min) ;
    return temp;
}

int GetRemainingTime(string a, string b)
{
    int atime = stoi(a.substr(0,2)) * 60 + stoi(a.substr(3,2));
    int btime = stoi(b.substr(0,2)) * 60 + stoi(b.substr(3,2));
    
    return atime - btime;
}

vector<string> solution(vector<vector<string>> plans) {
    vector<string> answer;
    stack<pair<string, int>> waitStack;
    
    // 시작 시간순으로 정렬
    sort(plans.begin(), plans.end(), compare);
    bool isStudying = false;
    for(int i = 0; i < plans.size()-1; i++)
    {
        isStudying = true;
        // 끝나눈 시간 구해서 시작시간과 비교
        string name = plans[i][0];
        string startTime = plans[i][1];
        string runTime = plans[i][2];
        string endTime = GetTime(startTime,runTime);
        
        string nextStart = plans[i+1][1];
        // 현재 과목 종료전에 다음 과목 시작될 경우
        if(CompareTime(endTime, nextStart))
        {
            waitStack.push(make_pair(name, GetRemainingTime(endTime, nextStart)));
        }
        else
        {
            
        }
    }
    
    return answer;
}
```
