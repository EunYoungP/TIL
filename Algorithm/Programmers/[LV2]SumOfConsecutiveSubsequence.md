2023.05.31

# __[프로그래머스 LV2] 연속 부분 수열 합__

투포인터

----

## __문제__

[연속 부분 수열 합](https://school.programmers.co.kr/learn/courses/30/lessons/178870)<br><Br>


## __나의 풀이__
```c++
#include <string>
#include <vector>
#include <queue>
#include <iostream>

using namespace std;

vector<int> solution(vector<int> sequence, int k) {
    vector<int> answer(2);
    vector<pair<int, int>> answers;
    
    // 합이 k인 가장짧은 부분 수열
    int start = 0;
    int end = 0;
    
    // 현재 부분 수열의 합 
    int temp = 0;
    
    // 현재 부분 수열
    queue<int> q;
    
    // 입력 수열 순회
    for(int i = 0; i < sequence.size(); i++)
    {
        int newNum = sequence[i];
        temp += newNum;
        end = i;
        
        q.push(newNum);

        if(temp > k)
        {
            while(temp > k)
            {
                int popNum = q.front();
                q.pop();
                temp -= popNum;
                start++;
            }
        }

        if(temp == k)
        {
            answers.push_back(make_pair(start, end));
        }
    }
    
    int min = sequence.size();
    for(int i = 0 ; i < answers.size(); i++)
    {
        int size = answers[i].second - answers[i].first;
        if(min > size)
        {
            min = size;
            answer[0] = answers[i].first;
            answer[1] = answers[i].second;
        }
    }
    return answer;
}
```

투포인터를 사용하여 문제를 해결했습니다.

입력값으로 주어진 수열을 순회하면서(O(logN)) 조건을 비교합니다.

해당 분기의 인덱스에 저장된 수열을 temp 변수에 누적해주며 end 포인터를 분기마다 증가시켜줍니다.

그리고 누적된 값이 k보다 크다면 큐에 누적된 수열의 값중 가장 작은 첫번째 값을 출력해주고 start 포인터를 증가시켜줍니다.

그리고 temp에 누적된 합이 k와 같아진다면 답 후보가 될 수 있는 answers 벡터에 start, end 값을 넣어줍니다.

마지막에 answers 벡터를 순회하면서 가장짧은 사이즈의 후보를 추출해냅니다.<br><Br>

주의해야 할 점은, 수열을 순회하는 반복문에서 한 분기에서 k==temp, k < temp 조건 검사를 모두 실행해야 합니다.

else if 문으로 하나의 조건만 검색한다면 모든 상황의 조건검사를 할 수 없습니다.


