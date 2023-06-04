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
<br><Br>

## __오류 수정__
```c++
#include <string>
#include <vector>
#include <queue>
#include <numeric>

using namespace std;

int solution(vector<int> queue1, vector<int> queue2) {
    // 큐의 원소의 합이 같게 하는 최소 작업 횟수
    // pop-insert => 1회
    long long answer = 0;
    queue<int> q1;
    queue<int> q2;
    
    // 1. 두 큐의 모든 요소의 합을 구합니다.
    long long sum1 = 0;
    long long sum2 = 0;
    
    for(int i = 0; i < queue1.size(); i++)
    {
        sum1 += queue1[i];
        sum2 += queue2[i];
    }
    
    if((sum1+sum2) % 2 == 1)
        return -1;
    
    int index1 = 0;
    int index2 = 0;
    while(true)
    {
        if(sum1 == sum2)
        {
            break;
        }
        else if(sum1 < sum2)
        {
            if(index2 < queue2.size())
            {
                sum1 += queue2[index2];
                sum2 -= queue2[index2];
            }
            else if(index2 < queue1.size() + queue2.size())
            {
                sum1 += queue1[index2 - queue2.size()];
                sum2 -= queue1[index2 - queue2.size()];
            }
            else
            {
                answer = -1;
                break;
            }
            index2++;
        }
        else if(sum1 > sum2)
        {
            if(index1 < queue1.size())
            {
                sum2 += queue1[index1];
                sum1 -= queue1[index1];

            }
            else if(index1 < queue1.size() + queue2.size())
            {
                sum2 += queue2[index1 - queue1.size()];
                sum1 -= queue2[index1 - queue1.size()];
            }
            else
            {
                answer = -1;
                break;
            }
            index1++;
        }
        answer++;
    }
    return answer;
}
```

큐에 직접 값을 대입하여 pop, push를 진행하는 방식에서 인덱스를 사용하여 배열에서 검색하는 방식으로 변경하였습니다.

이 두 큐의 최대 검색수는 queue1에서 모든 값을 pop하여 queue2에 넣고, 다시 queue2에서 하나만 제외한 값들을 queue1로 pop,push 하는 방법입니다.

따라서 최대 계산수는 queue1과 queue2의 주어진 개수가 동일하므로 ___queue1(queue2) * 3 -1___ 번입니다.

해당 회수를 초과하는 인덱스의 계산을 하는 경우는 -1을 반환해줍니다.<br><Br>

두 배열의 값의 합만 같아지면 되기때문에 굳이 배열에 값을 직접 넣거나 빼지않고,

따로 만든 sum1, sum2 변수에 각각 배열의 합을 구해서 인덱스로 접근한 배열의 값을 더하고 빼며 조건을 검사합니다.<br><Br>

주의할 점은 배열의 원소의 값이 10^9, 10억까지 가능하기 때문에 데이터 타입을 long long으로 변경해줘야합니다.



