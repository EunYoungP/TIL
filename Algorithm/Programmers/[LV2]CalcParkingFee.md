2023.06.05

# __[프로그래머스 LV2] 주차 요금 계산__

----

## __문제__

[주차 요금 계산](https://school.programmers.co.kr/learn/courses/30/lessons/92341)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <map>
#include <iostream>

using namespace std;

// 요금표, 출차 기록
vector<int> solution(vector<int> fees, vector<string> records) {
    // 차량별 주차 요금, 차량번호 작은 순
    vector<int> answer;
    
    // 1. 출차기록 문자열 정리[시각, 차량번호]
    map<int, vector<string>> m;
    for(int i = 0; i < records.size(); i++)
    {
        string time = records[i].substr(0,5);
        string carNum = records[i].substr(6,4);
    
        m[stoi(carNum)].push_back(time);
    }
    
    // 2. 차량별 이용 누적시간 구하기
    map<int, int> resultm;
    string endTime = "23:59";
    for(auto a : m)
    {
        int carNum = a.first;
        vector<string> curV = a.second;
        int inTime;
        int outTime;
        int total = 0;
        for(int j = 0; j < curV.size(); j+=2)
        {
            if(j+1 < curV.size())
            {
                inTime = stoi(curV[j].substr(0,2))*60 + stoi(curV[j].substr(3,2));
                outTime = stoi(curV[j+1].substr(0,2))*60 + stoi(curV[j+1].substr(3,2));
            }
            else
            {
                inTime = stoi(curV[j].substr(0,2))*60 + stoi(curV[j].substr(3,2));
                outTime = stoi(endTime.substr(0,2))*60 + stoi(endTime.substr(3,2));            
            }
            // 이용시간이 여러개일 경우를 대비하여 토탈값에 모두 더해줍니다.
            total += outTime - inTime;
        }
        resultm[carNum] += total;
    }
    
    // 3. 누적시간을 이용한 총 요금을 구합니다.
    int minTime = fees[0];
    int minFee = fees[1];
    int unitTime = fees[2];
    int unitFee = fees[3];
    for(auto a : resultm)
    {
        // 누적시간 <= 기본시간 : 기본요금
        if(a.second <= minTime)
            answer.push_back(minFee);
        // 누적시간 > 기본시간 : 기본요금 + 단위요금(초과 시간 올림)
        else if(a.second > minTime)
        {
            int lefttime = (a.second - minTime);
            if(lefttime % unitTime != 0)
                lefttime = (lefttime / unitTime + 1) * unitFee;
            else
                lefttime = (lefttime / unitTime) * unitFee;
            int pay = minFee + lefttime;
            answer.push_back(pay);
        }
    }
    return answer;
}
```

차량 번호 오름차순으로 답을 입력해야하기 때문에, 애초에 map이 기본으로 키값이 오름차순 정렬되는 점을 이용하여 map에 차량번호를 key값으로 넣어줬습니다.

그리고 value 값으로 해당 차량번호의 기록들을 벡터로 넣어줬습니다.<br><BR>

map의 키값을 알아내기 위해서 범위기반 반복문을 사용하여 first 키워드로 해당 차량번호에 해당하는 기록들을 이용한 총 이용시간을 구하여 담아줬습니다.

