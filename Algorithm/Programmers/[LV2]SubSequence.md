2023.04.21

# __[프로그래머스 LV2] 연속된 부분 수열의 합__



## __문제__

[연속된 부분 수열의 합](https://school.programmers.co.kr/learn/courses/30/lessons/178870)<br><Br>

## __나의 풀이__

```c#
#include <string>
#include <vector>
#include <queue>

using namespace std;

vector<int> solution(vector<int> sequence, int k) {
    vector<int> answer;
    
    // 합이 k인 가장짧은 부분 수열
    int start = 0;
    int end = 0;
    
    // 현재 부분 수열의 합 
    int temp = 0;
    
    // 현재 부분 수열
    queue<int> q;
    
    for(int i = 0; i < sequence.size(); i++)
    {
        int newNum = sequence[i];
        temp += newNum;
        end = i;
        
        if(temp < k)
        {
            q.push(newNum);
        }
        else if(temp > k)
        {
            int popNum = q.front();
            q.pop();
            temp -= popNum;
            start++;
        }
        else if(temp == k)
            break;
    }
    
    answer.push_back(start);
    answer.push_back(end);
    return answer;
}
```

시작과 끝을 의미하는 두 포인터를 생성하여 부분 수열의 합을 구하려고 했습니다.

하지만 테스트 

1 2 3 4 5 수열에서 합 7의 부분 수열을 구할 경우,

위 코드의 방법으로는 답인 [2, 3]이 구해지지 않고 

1 2 3 4 가 추가된 후 합이 k 보다 커져 1을 제거

2 3 4 5 가 또다시 합이 k 보다 커져 2를 제거

3 4 5 하는 방식으로 구해지지 않았습니다.

완벽한 정확성 테스트가 실행되지 않았습니다.

따라서 투 포인터를 사용하는 방법으로 참고하여 풀이하였습니다.



