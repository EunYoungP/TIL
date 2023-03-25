[2023.03.25](#나의-풀이참고)

# __[프로그래머스 LV3] 디스크 컨트롤러__

[프로그래머스_디스크컨트롤러](https://school.programmers.co.kr/learn/courses/30/lessons/42627)

우선순위큐/힙

---- 

<BR>

## __문제__

<img src = "https://user-images.githubusercontent.com/80774412/227724480-7f459d7f-3088-4784-8dfd-1a5bf2b47ddb.PNG"></img>

<img src = "https://user-images.githubusercontent.com/80774412/227724485-a1e99e05-4c51-40a9-9b90-c240825a1b2d.PNG"></img>

<img src = "https://user-images.githubusercontent.com/80774412/227724488-3ec96614-b66d-4956-aa32-b3f1ada07c4d.PNG"></img>

<img src = "https://user-images.githubusercontent.com/80774412/227724491-db7533db-a4a7-4eaf-b585-32af4ae0b4c0.PNG"></img>
<BR><BR>

## __나의 풀이__(참고)

```c++
#include <string>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

struct comp
{
    bool operator()(vector<int> a, vector<int> b)
    {
        return a.at(1) > b.at(1);
    }
};

// 작업 요청 시점, 작업 소요시간
int solution(vector<vector<int>> jobs) {
    // 작업 요청~종료 시간 평균 최소값
    int answer = 0;
    int time = 0;
    int curIndex = 0;
    
    priority_queue<vector<int>, vector<vector<int>>, comp> pq;
    
    // 작업시간 짧은 순 정리
    sort(jobs.begin(), jobs.end());
    
    while(!pq.empty() || jobs.size() > curIndex)
    {
        // 대기 중인 작업 큐에 추가
        if(jobs[curIndex][0] <= time)
        {
            pq.push(jobs[curIndex++]);
            // 같은 시간에 추가되는 작업이 있는지 다시 검사
            continue;
        }
        
        // 이미 실행중인 작업이 있는 상태
        if(!pq.empty())
        {
            time += pq.top()[1];
            
            answer += time-pq.top()[0];
            
            pq.pop();
        }
        // 실행중인 작업이 없다면 다음 요소의 작업 추가 시간으로 넘깁니다.
        else
        {
            time +=jobs[curIndex][0];
        }
    }
    return answer/jobs.size();
}
```

우선순위가 높은 요소가 먼저 출력되는 우선순위큐를 이용해서 작업시간이 짧은 작업을 먼저 출력할 수 있습니다.

```
// 우선순위큐 선언 방법

priority_queue<자료형, 구현체, 비교연산자> pq;
```