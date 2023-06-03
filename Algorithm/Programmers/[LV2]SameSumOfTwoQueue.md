2023.06.03

# __[프로그래머스 LV2] 두 큐 합 같게 만들기__

----

## __문제__

[두 큐 합 같게 만들기](https://school.programmers.co.kr/learn/courses/30/lessons/118667#qna)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <queue>

using namespace std;

int solution(vector<int> queue1, vector<int> queue2) {
    // 큐의 원소의 합이 같게 하는 최소 작업 횟수
    // pop-insert => 1회
    int answer = -2;
    queue<int> q1;
    queue<int> q2;
    
    // 1. 두 큐의 모든 요소의 합을 구합니다.
    int total = 0;
    int sum1 = 0;
    int sum2 = 0;
    for(int i = 0; i < queue1.size(); i++)
    {
        sum1 += queue1[i];
        total += queue1[i];
        q1.push(queue1[i]);
        sum2 += queue2[i];
        total += queue2[i];
        q2.push(queue2[i]);
    }
    
    // 같은 합으로 나눌 수 없는 경우 예외처리
    if(total % 2 == 1)
        return -1;
    
    int calcNum = 0;
    while(sum1 != sum2)
    {
        if(sum1 < sum2)
        {
            int temp = q2.front();
            q2.pop();
            sum2 -= temp;
            q1.push(temp);
            sum1 += temp;
            calcNum++;
        }
        else if(sum1 > sum2)
        {
            int temp = q1.front();
            q1.pop();
            sum1 -= temp;
            q2.push(temp);
            sum2 += temp;
            calcNum++;
        }
        else if(sum1 == sum2)
        {
            answer = calcNum;
            break;
        }
    }
    return answer;
}
```

