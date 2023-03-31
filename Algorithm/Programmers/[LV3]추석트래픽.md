[2023.03.31]

# __[프로그래머스 LV3] 추석 트래픽__

DP

---- 

<BR>

<BR><BR>


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
        
        double endTime = (stod(temp.substr(0,2)) * 3600) + (stod(temp.substr(3,2)) * 60) + stod(temp.substr(6,2)) + (stod(temp.substr(9)) / 1000.0);
        
        temp = a.substr(24);
        double throughtTime = stod(temp.substr(0, temp.size()-1));
        double startTime = endTime - throughtTime  + 0.001;
        
        timelines.push_back(make_pair(startTime, endTime));
    }
    
    for(int i= 0; i < timelines.size(); i++)
    {
        double start = timelines[i].first;
        double end = timelines[i].second;
        
        num = 1;
        for(int j= i+1; j < timelines.size(); j++)
        {
            if(start + 1 >= timelines[j].first || end + 1 > timelines[j].first)
                num++;
        }
        answer = max(num, answer);
    }
    return answer;
}
```