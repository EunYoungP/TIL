2023.06.02

# __[프로그래머스 LV2] 우박수열 정적분__



----

## __문제__

[우박수열 정적분](https://school.programmers.co.kr/learn/courses/30/lessons/134239#qna)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <iostream>

using namespace std;

// 사다리꼴의 넓이를 구합니다.
double GetTrapezoidArea(int upperSide, int lowerSide, int height)
{
    return (double)((upperSide + lowerSide) * height) / 2;
}

// 우박수 초항, 정적분 구간 목록
vector<double> solution(int k, vector<vector<int>> ranges) {
    // 우박수, 우박수열, 정적분 결과 목록
    // 시작점 > 끝점 => -1
    vector<double> answer;
    vector<int> collatz;
    
    // 1.k의 우박수열을 구합니다.
    collatz.push_back(k);
    while(k!=1)
    {
        if(k%2 == 0)
        {
            k/=2;
        }
        else
        {
            k=k*3+1;
        }
        collatz.push_back(k);
    }
    
    // 2. 구간 목록을 순회하며 넓이를 구합니다.
    for(int i= 0; i < ranges.size(); i++)
    {
        int startIndex = ranges[i][0];
        int endIndex = collatz.size() -1 + ranges[i][1];
        
        if(startIndex > endIndex)
        {
            answer.push_back(-1);
            continue;
        }
        
        double temp = 0;
        for(int j = startIndex; j < endIndex; j++)
        {
            temp += GetTrapezoidArea(collatz[j], collatz[j+1], 1);
        }
        answer.push_back(temp);
    }
    return answer;
}
```

__문제풀이__

1. 입력값 k의 __우박수열__ 을 구합니다. k가 1이 될때까지 아래 계산을 진행합니다.

    1-1. 만약 k가 짝수라면 k /2

    1-2. 만약 k가 홀수라면 k*3 + 1<br><Br>

2. ranges 목록을 순회하면서 각 구간의 넓이를 구합니다.

    2-1. ranges에서 start인덱스와 end인덱스를 구해줍니다.

    2-2. 만약 start > end라면 -1을 입력해줍니다.

    2-3. 만약 start 와 end의 차이가 1보다 크다면 한 구간씩 사디리꼴의 넓이를 구해줍니다.

    <img src="https://github.com/EunYoungP/TIL/assets/80774412/55665f98-86a9-497b-b0cc-116db2ac3dab" width=400></img>

    <k가 5일 경우의 우박수열 그래프>

    노란색으로 채워진 부분처럼 사다리꼴로 넓이를 구할 수 있습니다.

