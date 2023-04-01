[2023.03.31]

# __[프로그래머스 LV3] 추석 트래픽__



## __문제__

[프로그래머스_추석 트래픽](https://school.programmers.co.kr/learn/courses/30/lessons/17676?language=cpp)<br><Br>

## __나의 풀이__(참고)
```c++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int solution(vector<string> lines) {
    int answer = 0;
    
    int num;
    vector<pair<double,double>> timelines;
    
    for(auto a : lines)
    {
        string temp = a.substr(11,12);
        
        // 트래픽 응답완료 시간:분:초 를 초 단위로 모두 바꿔줍니다.
        double endTime = (stod(temp.substr(0,2)) * 3600) + (stod(temp.substr(3,2)) * 60) + stod(temp.substr(6,2)) + (stod(temp.substr(9)) / 1000.0);
        
        temp = a.substr(24);
        double throughtTime = stod(temp.substr(0, temp.size()-1));
        double startTime = endTime - throughtTime  + 0.001;
        
        // 트래픽 요청 시간, 트래픽 응답완료 시간
        timelines.push_back(make_pair(startTime, endTime));
    }
    
    for(int i= 0; i < timelines.size(); i++)
    {
        double start = timelines[i].first;
        double end = timelines[i].second;
        
        num = 1;
        for(int j= i+1; j < timelines.size(); j++)
        {
            // 다음 인덱스의 응답완료 시간이, 전 트래픽 시작시간보다 1초 이하라면 동시에 처리중입니다.
            // 다음 인덱스의 시작 시간이 
            if(start + 1 >= timelines[j].first || end + 1 > timelines[j].first)
                num++;
        }
        answer = max(num, answer);
    }
    return answer;
}
```