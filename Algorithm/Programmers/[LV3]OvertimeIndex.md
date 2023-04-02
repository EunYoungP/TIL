2023.04.02

# __[프로그래머스 LV3] 야근 지수__

queue

## __문제__

[야근 지수](https://school.programmers.co.kr/learn/courses/30/lessons/12927)<br><Br>

## __나의 풀이__
```c++
#include <vector>
#include <queue>
using namespace std;

long long solution(int n, vector<int> works) {
    // works 값들을 queue에 캐싱
    std::priority_queue<int> q(works.begin(), works.end());
    for (int i = 0; i < n; i++) {
        if (q.top() > 0) {
            int tmp = q.top();
            q.pop();
            q.push(tmp-1);
        }
    }
    long long answer = 0;
    while(!q.empty()) {
        answer += (long long)q.top() * (long long)q.top();
        q.pop();
    }
    return answer;
}
```

우선순위 큐를 사용해 값들을 자동 정렬하여 문제를 풀이했습니다.

